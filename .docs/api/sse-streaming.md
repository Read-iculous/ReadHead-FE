# SSE Streaming Protocol — ReadHead-FE

## 1. Overview
ReadHead-FE receives AI completions via Server-Sent Events (SSE) from `POST /api/v1/ai/stream`.
Because native `EventSource` in web browsers only supports `GET` requests without request body, ReadHead-FE implements an SSE reader using `window.fetch` + `response.body.getReader()`.

---

## 2. Event Types & Payloads

The backend emits typed SSE events separated by double newlines (`\n\n`):

### `event: start`
Emitted immediately upon initiating the stream, providing session identifiers.
```
event: start
data: {"conversation_id": "conv-uuid", "message_id": "msg-uuid"}
```

### `event: token`
Emitted repeatedly as the model generates text.
```
event: token
data: {"content": "Machine ", "index": 0}

event: token
data: {"content": "learning ", "index": 1}
```

### `event: citation`
Emitted when the AI identifies a fact grounded in the page DOM (BR-12~14).
```
event: citation
data: {"source_id": "p-14", "text_snippet": "Neural networks process data in layers", "is_grounded": true, "confidence": 0.95}
```

### `event: done`
Signals normal stream completion with metadata.
```
event: done
data: {"message_id": "msg-uuid", "finish_reason": "stop", "total_tokens": 142, "provider": "gemini", "model": "gemini-1.5-flash"}
```

### `event: error`
Emitted if an upstream AI provider, key, or quota fault occurs.
```
event: error
data: {"code": "UPSTREAM_QUOTA_EXCEEDED", "detail": "Your Gemini API quota has been exhausted. Check Google AI Studio."}
```

---

## 3. Client Implementation Pattern (`src/shared/services/sseStreamReader.ts`)

```ts
export async function readSSEStream(
  url: string,
  payload: StreamPayload,
  token: string,
  callbacks: {
    onStart: (meta: { conversation_id: string; message_id: string }) => void;
    onToken: (chunk: string) => void;
    onCitation: (citation: Citation) => void;
    onError: (err: { code: string; detail: string }) => void;
    onDone: (meta: StreamDoneMeta) => void;
  },
  abortSignal?: AbortSignal
) {
  const response = await fetch(url, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': `Bearer ${token}`
    },
    body: JSON.stringify(payload),
    signal: abortSignal
  });

  if (!response.ok || !response.body) {
    const errorBody = await response.json().catch(() => ({}));
    throw new Error(errorBody.detail || 'Failed to start AI stream');
  }

  const reader = response.body.getReader();
  const decoder = new TextDecoder('utf-8');
  let buffer = '';

  while (true) {
    const { done, value } = await reader.read();
    if (done) break;

    buffer += decoder.decode(value, { stream: true });
    const events = buffer.split('\n\n');
    buffer = events.pop() || ''; // Keep partial chunk in buffer

    for (const rawEvent of events) {
      if (!rawEvent.trim()) continue;
      const lines = rawEvent.split('\n');
      let eventType = 'token';
      let dataStr = '';

      for (const line of lines) {
        if (line.startsWith('event: ')) eventType = line.substring(7).trim();
        if (line.startsWith('data: ')) dataStr = line.substring(6).trim();
      }

      if (!dataStr) continue;
      const parsed = JSON.parse(dataStr);

      if (eventType === 'start') callbacks.onStart(parsed);
      else if (eventType === 'token') callbacks.onToken(parsed.content);
      else if (eventType === 'citation') callbacks.onCitation(parsed);
      else if (eventType === 'done') callbacks.onDone(parsed);
      else if (eventType === 'error') callbacks.onError(parsed);
    }
  }
}
```

## 4. Connection Interruption & Error Policy
- **Stream Termination on Error**: Upon receiving `event: error`, the reader terminates immediately. The backend **never** emits `event: done` following an error.
- **Client-Side Abort (Panel Close / Tab Switch)**: When the user cancels via `AbortController`, tokens already rendered in the Side Panel are kept intact in the conversation timeline with an `interrupted` badge and an inline "Retry" button. Both client and server preserve the partial response.

---

## 5. Concurrency & Rate Limiting Guardrails
- **Max Active Streams**: The client and server enforce a maximum of **2 active concurrent SSE streams** per user. Initiating a 3rd stream returns `429 STREAM_CONCURRENCY_EXCEEDED`.
- **Handshake Rate Limit**: `slowapi` applies strictly to the initial HTTP POST handshake (20 requests/minute). Streaming individual tokens does not deplete request quota.
- **Client-side PDF Extraction**: For PDF documents, the client extracts clean text locally using Mozilla `pdf.js` (max 100 KB) and sends `content_type: "pdf_text"`. Binary PDF files are never uploaded to the backend.
