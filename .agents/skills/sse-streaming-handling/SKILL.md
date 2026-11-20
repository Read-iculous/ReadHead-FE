---
name: sse-streaming-handling
description: Workflow for handling, rendering, and debugging real-time Server-Sent Events AI streams in ReadHead-FE.
---

# SSE Streaming Handling Skill

## When to Use
Use when implementing or debugging the real-time AI response stream in the Extension Side Panel or Web Dashboard.

## Step-by-Step Procedure

1. **Verify Stream Endpoint**:
   - Backend endpoint: `POST /api/v1/ai/stream`.
   - Protocol details documented in `.docs/api/sse-streaming.md`.

2. **Setup Stream Consumer**:
   - Utilize `readSSEStream` from `src/shared/services/sseStreamReader.ts`.
   - Provide an `AbortController` signal to allow the user to stop generation mid-stream.

3. **Handle Incoming Stream Events**:
   - `onToken`: Append text chunks to the active message state in real-time.
   - `onCitation`: Store grounding citations for interactive badge rendering.
   - `onDone`: Finalize the message, store message metadata, and re-enable input box.
   - `onError`: Display descriptive error banner without discarding previously streamed text.

4. **Verify Markdown & Math Rendering**:
   - Stream content through `react-markdown` with `rehype-katex` to ensure math formulas and code blocks render cleanly as tokens arrive.
