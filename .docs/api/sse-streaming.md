# SSE Streaming Protocol — ReadHead-FE

## 1. Overview
ReadHead-FE receives AI completions via Server-Sent Events (SSE) from `POST /api/v1/ai/stream`.
Because native `EventSource` in web browsers only supports `GET` requests without request body, ReadHead-FE implements an SSE reader using `window.fetch` + `response.body.getReader()`.

---

## 2. Event Types & Payloads

The backend emits typed SSE events separated by double newlines (`\n\n`):

### `event: token`
Emitted repeatedly as the model generates text.
```
event: token
data: {"content": "Machine ", "index": 0}

event: token
data: {"content": "learning ", "index": 1}
```

### `event: citation`
Emitted when the AI identifies a fact grounded in the page DOM.
```
event: citation
data: {"source_id": "p-14", "text_snippet": "Neural networks process data in layers", "confidence": 0.95}
```

### `event: grounding_status`
Distinguishes between facts found in the page vs external AI knowledge (SRS BG-07).
```
event: grounding_status
data: {"is_grounded": true, "unverified_claims": []}
```

### `event: done`
Signals normal stream completion with metadata.
```
event: done
data: {"message_id": "msg_987", "total_tokens": 142, "provider": "gemini", "model": "gemini-1.5-flash"}
```

### `event: error`
Emitted if an upstream AI provider or quota fault occurs.
```
event: error
data: {"error_code": "KEY_QUOTA_EXCEEDED", "detail": "Your Gemini API quota has been exhausted. Check Google AI Studio."}
```

---

## 3. Client Implementation Pattern (`src/shared/services/sseStreamReader.ts`)

```ts
export async function readSSEStream(
  url: string,
  payload: StreamPayload,
  token: string,
  callbacks: {
    onToken: (chunk: string) => void;
    onCitation: (citation: Citation) => void;
    onError: (err: { error_code: string; detail: string }) => void;
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

      if (eventType === 'token') callbacks.onToken(parsed.content);
      else if (eventType === 'citation') callbacks.onCitation(parsed);
      else if (eventType === 'done') callbacks.onDone(parsed);
      else if (eventType === 'error') callbacks.onError(parsed);
    }
  }
}
```
