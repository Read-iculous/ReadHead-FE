# Architecture Decisions (ADRs) — ReadHead-FE

## ADR-01: Dual Delivery with Shared Core
- **Context**: The project requires both a Chrome Extension (Manifest V3) for reading assistance and a Web Management Portal for system administration and user analytics.
- **Decision**: Structure `ReadHead-FE` with a shared core (`src/shared/`) housing common UI components, API clients, stores, and TypeScript types, powering two build outputs (`src/extension/` and `src/web/`).
- **Consequence**: Zero code duplication for API contracts and styling, while isolating Chrome-specific APIs from the standard web portal.

## ADR-02: Manifest V3 & Vite Build Engine
- **Context**: Chrome strictly enforces Manifest V3. Bundling must support background service workers, content script CSS isolation, and side panel HTML.
- **Decision**: Use Vite 5+ with `@crxjs/vite-plugin`.
- **Consequence**: Provides instant HMR during development, generates clean Manifest V3 compliant output, and eliminates the legacy webpack configuration overhead.

## ADR-03: Shadow DOM for In-Page Content Script UI
- **Context**: Injected UI elements (floating tooltip, text action bubble) must coexist with arbitrary third-party website stylesheets without style collisions.
- **Decision**: Encapsulate all injected content script UI inside a closed Shadow DOM container (`attachShadow({ mode: 'closed' })`).
- **Consequence**: Guarantees pixel-perfect styling isolated from host CSS resets or overriding styles.

## ADR-04: Zustand for Synchronous UI State + TanStack Query for Server State
- **Context**: Frontend requires both immediate, synchronous state (active tab switching, draft inputs, UI drawer toggles) and robust cached server data (conversations, BYOK keys, user profile).
- **Decision**: Use **Zustand** for lightweight client-side state and **TanStack Query** (React Query v5) for server cache, auto-refetch, and optimistic updates.
- **Consequence**: Clear mental model: Zustand holds client UI state; TanStack Query manages backend server state.

## ADR-05: SSE Streaming via Native Fetch + ReadableStream
- **Context**: Standard `EventSource` only supports `GET` requests and cannot pass custom request body or headers in standard browser APIs. ReadHead-BE expects `POST /api/v1/ai/stream` with a JSON payload and Bearer Authorization header.
- **Decision**: Implement an SSE reader using `window.fetch` and `response.body.getReader()`.
- **Consequence**: Full support for `POST` payloads, custom auth headers, real-time chunk consumption, and clean cancellation via `AbortController`.

## ADR-06: Local-First Heuristic for Proactive Reading Support (Feature G)
- **Context**: User attention monitoring (dwell time, reading pace) could either stream continuous user telemetry to the server or execute locally in the browser.
- **Decision**: Execute monitoring strictly on the client using lightweight heuristics (paragraph dwell timer, scroll delta, readability indices).
- **Consequence**: Zero latency, total user privacy, no server compute waste, and zero risk of streaming sensitive unread page content across the wire.
