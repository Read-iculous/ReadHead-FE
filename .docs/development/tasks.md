# ReadHead-FE — Frontend Implementation Task Breakdown

This document defines the complete frontend implementation roadmap derived from BRS, SRS, SyRS, OpsCon, `.docs/`, and `.agents/rules/`. Every task adheres to a standardized senior-grade specification format: **Task Name & ID**, **Feature Reference**, **Description**, **Plan & Codebase**, **Scenarios & Edge Cases**, **Senior 4-Lens Validation** (👔 BA, 🛠️ TechLead, 🚀 DevOps, 🏛️ Software Architecture), **Acceptance Criteria**, and **Verification / Screenshot**.

---

## Roadmap Overview

```
Phase 1: Project Scaffolding & Core Shared Infrastructure
  ├── [FE-01] Multi-Target Vite Scaffolding, TypeScript Config & Manifest V3 Engine (TASK-FE-101, 102)
  ├── [FE-02] Design System, Tailwind CSS & Accessible Shadow-Compatible UI Primitives (TASK-FE-103)
  ├── [FE-03] Domain TypeScript Contract & Typed Axios Client with JWT Lifecycle (TASK-FE-104, 105)
  ├── [FE-04] Native Fetch SSE Streaming Engine, Parser & Concurrency Guard (TASK-FE-106)
  └── [FE-05] Global State Architecture (Zustand) & Storage Abstraction Layer (TASK-FE-107, 108)

Phase 2: Chrome Extension Surfaces & In-Page Injection (Manifest V3)
  ├── [FE-06] Background Service Worker, Chrome Lifecycle & Cross-Context Bus (TASK-FE-201)
  ├── [FE-07] Closed Shadow DOM Floating Action Menu & Selection Watcher (TASK-FE-202)
  ├── [FE-08] In-Page Article Extractor, SPA Interceptor & PDF Text Ingestion (TASK-FE-203)
  ├── [FE-09] Non-Destructive In-Page Accessibility Modifiers (OpenDyslexic & Bionic) (TASK-FE-204)
  └── [FE-10] Local-First Dwell Time Tracker & Proactive Reading Cues Engine (TASK-FE-205)

Phase 3: Chrome Side Panel, Chat Interaction & Comprehension Quizzes
  ├── [FE-11] Chrome Side Panel Workspace, Tab Isolation & AI Model Switcher (TASK-FE-206a)
  ├── [FE-12] Real-Time Streaming Chat, KaTeX Math & Interactive Citations (TASK-FE-206b)
  ├── [FE-13] Interactive Comprehension Quiz Card Subsystem & Evaluation Review (TASK-FE-207)
  ├── [FE-14] AI Response Feedback & Accuracy Reporting Widget (TASK-FE-206c)
  └── [FE-15] Action Toolbar Popup & Options Page (BYOK Vault & Settings) (TASK-FE-208, 209)

Phase 4: Privacy Guardrails, Offline Resilience & Multi-Device Sync
  ├── [FE-16] Client-Side Pre-Flight Sensitive Content Inspector & Domain Privacy Gate (TASK-FE-301, 303, 304)
  ├── [FE-17] Continuous Prompt Draft Autosave, Offline Resilience & Session Purge (TASK-FE-302, 305)
  ├── [FE-18] Multi-Device Incremental Sync Client, Conflict Resolution & Offline Outbox (TASK-FE-306, 307)
  └── [FE-19] In-Page Web Highlights Manager & Dynamic SPA Re-Hydration (TASK-FE-203b)

Phase 5: Web Management Portal (User Dashboard, Admin Telemetry & Documentation)
  ├── [FE-20] Web Portal Application Scaffolding & User Reading Analytics Dashboard (TASK-FE-401, 402)
  ├── [FE-21] Admin Portal Telemetry, User Moderation & Quality Feedback Review (TASK-FE-403)
  └── [FE-22] Static Documentation, BYOK Handbook & Help Center (TASK-FE-404)

Phase 6: Quality Assurance, End-to-End Testing & Extension Packaging
  ├── [FE-23] Comprehensive Unit & Integration Test Suite (Vitest & RTL) (TASK-FE-501, 502)
  └── [FE-24] Production Bundle Optimization, Security Audit & Web Store Packaging (TASK-FE-503, 504)
```

---

## Phase 1: Project Scaffolding & Core Shared Infrastructure

### [FE-01] Multi-Target Vite Scaffolding, TypeScript Config & Manifest V3 Engine
- **Task ID & Feature Reference**: `TASK-FE-101`, `TASK-FE-102` | Feature L (`FR-OPS-01~05`), `ADR-01`, `ADR-02`, `NFR-MNT-01`
- **Description**: Configure a dual-build repository producing two distinct artifacts from a shared core (`src/shared/`):
  1. Chrome Extension (Manifest V3) running Background Service Worker, Content Scripts, Side Panel, Popup, and Options.
  2. Standalone Web Management Portal SPA for user analytics, documentation, and platform admin.
- **Plan & Codebase**:
  - `package.json`: Pin dependencies (`react@^18.2.0`, `vite@^5.1.4`, `@crxjs/vite-plugin@^2.0.0-beta.23`, `typescript@^5.3.3`, etc.).
  - `tsconfig.json` & `tsconfig.node.json`: Strict TypeScript mode, path aliases (`@/shared/*`, `@/extension/*`, `@/web/*`).
  - `vite.config.ts`: Multi-configuration build pipeline. Use `@crxjs/vite-plugin` for extension builds (`manifest.json`) and a dedicated build target for the Web Portal.
  - `manifest.json`: Manifest V3 specification:
    - Permissions: `activeTab`, `sidePanel`, `storage`, `scripting`, `alarms`.
    - Host permissions: `https://*/*`, `http://*/*`.
    - `side_panel`: `{"default_path": "src/extension/sidepanel/index.html"}`.
    - `background`: `{"service_worker": "src/extension/background/index.ts", "type": "module"}`.
    - `web_accessible_resources`: Expose `assets/fonts/*.woff2` and extension icons for all origins (`<all_urls>`).
- **Scenarios & Edge Cases**:
  - *Dev HMR in Extension*: `@crxjs/vite-plugin` WebSocket reconnects reliably when modifying React components in Side Panel without killing background worker.
  - *Host CSP Font Block*: Webpages with strict Content Security Policy (`font-src 'self'`) fail to load remote fonts; bundling fonts inside `web_accessible_resources` and accessing via `chrome-extension://` bypasses host restrictions.
  - *Build Artifact Segregation*: Running `npm run build:extension` outputs strictly to `dist/extension/`; `npm run build:web` outputs to `dist/web/`.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Guarantees zero friction installation for end users, meeting Chrome Web Store compliance requirements from day one.
  - 🛠️ **TechLead Lens**: Strict TypeScript configuration (`noImplicitAny`, `strictNullChecks`) and path alias hygiene eliminate cross-context import errors.
  - 🚀 **DevOps Lens**: Multi-target Vite configuration enables unified repository management with separate CI artifact outputs and independent cache invalidation.
  - 🏛️ **Software Architecture Lens**: Concrete enforcement of `ADR-01` (Dual Delivery with Shared Core) ensuring business logic and API contracts in `src/shared/` are never contaminated with Chrome-specific extension globals.
- **Acceptance Criteria**:
  - `npm run build` cleanly builds both the extension bundle (`dist/extension`) and web portal (`dist/web`) without TypeScript errors.
  - Loading `dist/extension` as an unpacked extension in `chrome://extensions` generates zero manifest errors or warnings.
  - Service Worker registers as active ES module; Side Panel opens successfully via Chrome extension toolbar.
- **Verification / Screenshot**:
  - Command: `npm run build && npm run type-check`
  - Screenshot: Chrome `chrome://extensions` screen showing "ReadHead" loaded unpacked with ID, permissions, and active background worker.

---

### [FE-02] Design System, Tailwind CSS & Accessible Shadow-Compatible UI Primitives
- **Task ID & Feature Reference**: `TASK-FE-103` | Feature B (`FR-CFG-01~07`), `ADR-03`, `NFR-ACC-01~04`
- **Description**: Configure Tailwind CSS, PostCSS, and accessible UI component primitives (Shadcn/ui & Radix UI). Ensure style encapsulation works seamlessly both in standard DOM (Side Panel & Web Portal) and within the closed Shadow DOM injected into host webpages.
- **Plan & Codebase**:
  - `tailwind.config.ts`: Configure palette (ReadHead indigo/violet, accessibility high-contrast tokens, dark mode classes).
  - `postcss.config.js`: Setup autoprefixer and Tailwind.
  - `src/shared/styles/globals.css`: Base styles, typography variables, CSS reset.
  - `src/shared/styles/shadow-dom.css`: Scoped CSS bundle compiled specifically for injection into `attachShadow({ mode: 'closed' })` without leaking into host page or being affected by host CSS resets.
  - `src/shared/components/ui/`: Radix-based primitives: Button, Dialog, Dropdown, Tooltip, Switch, Tabs, Badge, Progress, Toast.
- **Scenarios & Edge Cases**:
  - *Host CSS Reset Interference*: Aggressive host CSS resets (e.g. `* { all: unset; box-sizing: content-box; }` on Wikipedia or Medium) attempt to distort extension buttons; `:host { all: initial; }` and closed shadow boundary nullify external influence.
  - *Z-Index Wars*: Webpages with floating navbars at `z-index: 2147483647`; Shadow container mounts at max 32-bit signed int `z-index: 2147483647` with viewport boundary collision detection.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Meets WCAG 2.1 AA standards for color contrast ratio (minimum 4.5:1 for normal text, 7:1 for high-contrast mode), providing accessible UX for dyslexic and visually impaired readers.
  - 🛠️ **TechLead Lens**: Modular Radix UI primitives prevent accessible keyboard navigation bugs (`aria-expanded`, keyboard focus trap, `Esc` dismiss).
  - 🚀 **DevOps Lens**: Tailwind purged output generates minimal CSS footprint (< 30KB gzip) to maintain lightning-fast Content Script injection times.
  - 🏛️ **Software Architecture Lens**: Adheres to `ADR-03`, strictly isolating in-page UI via closed Shadow DOM while sharing underlying design tokens across Extension and Web.
- **Acceptance Criteria**:
  - Tailwind utilities render identically in standalone web pages and inside closed Shadow DOM containers.
  - All UI primitives pass keyboard accessibility tests (Tab order, Enter/Space activation, Escape dismissal).
  - Dark mode and High Contrast themes toggle instantly via CSS variable re-binding.
- **Verification / Screenshot**:
  - Command: `npm run storybook` or mounting UI test harness.
  - Screenshot: Side Panel rendering all UI components (buttons, switches, dialogs) alongside an injected host page showing identical floating UI styling.

---

### [FE-03] Domain TypeScript Contract & Typed Axios Client with JWT Lifecycle
- **Task ID & Feature Reference**: `TASK-FE-104`, `TASK-FE-105` | Feature A (`FR-ACC-01~06`, `FR-SEC-01~03`), `FR-ERR-01~04`
- **Description**: Define TypeScript data contracts matching ReadHead-BE Pydantic schemas. Implement a typed Axios HTTP client equipped with automatic JWT token management: clock-drift leeway, proactive refresh, concurrent request queuing on 401, and immediate multi-tab session purge upon revocation.
- **Plan & Codebase**:
  - `src/shared/types/api.ts`: Response envelopes (`ApiResponse<T>`, `ApiErrorResponse`, `PaginatedResponse<T>`).
  - `src/shared/types/auth.ts`: `User`, `AuthTokens`, `LoginPayload`, `RegisterPayload`, `OAuthPayload`.
  - `src/shared/types/settings.ts`: `UserSettings`, `AccessibilityProfile`.
  - `src/shared/services/apiClient.ts`:
    - Base URL configured via `VITE_API_BASE_URL`.
    - Request interceptor: Appends `Authorization: Bearer <access_token>` from `authStore`.
    - Proactive refresh timer: Detects token expiry and refreshes 3 minutes before `expires_in` (accounting for 60s server clock drift).
    - Response interceptor: On `401 Unauthorized`:
      - If error code is `USER_SUSPENDED` or `REFRESH_TOKEN_REVOKED`: Purges local tokens immediately and redirects to login.
      - Standard 401: Queues concurrent pending requests in a subscriber list while executing a single `POST /api/v1/auth/refresh` call. Dispatches queued calls once new access token is acquired within the 30-second concurrency grace period.
- **Scenarios & Edge Cases**:
  - *Multi-Tab Refresh Race*: 4 browser tabs fire simultaneous requests as the access token expires. Only one `/auth/refresh` call is made; the remaining 3 wait and replay with the new token.
  - *Suspended Account*: Server returns `401 USER_SUSPENDED`. Client aborts all pending retries, clears storage, and broadcasts an alert banner.
  - *Offline / Network Dropout*: Client catches network disconnect (`ERR_NETWORK_CHANGED` or offline), halts automated retry storm, and shows a non-intrusive offline toast.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Seamless user experience—users are never abruptly booted out to the login screen while actively reading or drafting notes.
  - 🛠️ **TechLead Lens**: Queue-based subscriber pattern in Axios interceptor prevents token refresh stampedes and race conditions.
  - 🚀 **DevOps Lens**: Reduces backend load and Redis grace period pressure by deduplicating refresh calls on the client side.
  - 🏛️ **Software Architecture Lens**: Enforces strict backend contract parity with `ReadHead-BE` (`/api/v1/*`), ensuring zero schema drift between Python Pydantic models and TypeScript types.
- **Acceptance Criteria**:
  - Requests automatically include Bearer token.
  - Expired access tokens trigger silent token rotation without user interruption.
  - Simultaneous 401 requests are cleanly resolved with exactly one `/auth/refresh` call.
  - Terminal 401s (`USER_SUSPENDED`, `REFRESH_TOKEN_REVOKED`) immediately clear auth state and navigate to `/login`.
- **Verification / Screenshot**:
  - Test: `npm run test src/shared/services/__tests__/apiClient.test.ts`
  - Screenshot: Chrome DevTools Network tab showing 3 concurrent 401 requests, followed by 1 `/auth/refresh` (200 OK), and automatic 200 OK replay of the original 3 requests.

---

### [FE-04] Native Fetch SSE Streaming Engine, Parser & Concurrency Guard
- **Task ID & Feature Reference**: `TASK-FE-106` | Feature E (`FR-AI-01~12`), `ADR-05`, `ADR-09`, `ISSUE-001`
- **Description**: Build an industrial-grade Server-Sent Events (SSE) streaming engine using native `fetch` and `ReadableStream.getReader()`. Supports `POST` payloads with JWT authentication, typed event parsing (`start`, `token`, `citation`, `done`, `error`), client-side cancellation via `AbortController` (preserving partial outputs), and a strict client concurrency guard limiting active streams to max 2.
- **Plan & Codebase**:
  - `src/shared/services/sseStreamReader.ts`:
    - Core class `SSEStreamReader` managing the HTTP connection, UTF-8 chunk decoding, and SSE line buffering (`event:`, `data:`).
    - Dispatches typed callbacks: `onStart(meta)`, `onToken(token)`, `onCitation(citation)`, `onDone(summary)`, `onError(error)`.
  - `src/shared/hooks/useAIStream.ts`: React hook encapsulating streaming lifecycle, live text buffer accumulation, typing cursor state, and abort triggers.
  - Enforce execution within UI document contexts (Side Panel / Popup) rather than Background Service Worker, eliminating MV3 30-second worker termination risks.
  - Track active stream count globally: reject new stream requests with client toast if 2 streams are already running.
- **Scenarios & Edge Cases**:
  - *Fragmented Multi-Byte Chunks*: Multi-byte UTF-8 characters (e.g. Vietnamese accents, emoji) split across chunk boundaries are safely buffered and decoded without replacement glyph errors (``).
  - *Mid-Stream User Cancellation*: User clicks "Stop Generating". `AbortController.abort()` cleanly severs the socket; partial text received up to that millisecond is preserved in chat history; backend is notified to abort upstream LLM inference (`ISSUE-001`).
  - *Server 429 Concurrency Exceeded*: Backend returns `429 STREAM_CONCURRENCY_EXCEEDED`; engine surfaces a clear message: *"You have reached the maximum of 2 simultaneous streaming requests. Please wait for one to finish."*
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Gives readers real-time feedback with instant perceived responsiveness (< 500ms TTFT) and complete control to stop generation at will.
  - 🛠️ **TechLead Lens**: Chunk boundary buffer assembly prevents malformed JSON payloads in event data; unbinds event listeners on React component unmount.
  - 🚀 **DevOps Lens**: Client-side cancellation immediately frees backend FastAPI connection pool slots and prevents wasted upstream LLM tokens.
  - 🏛️ **Software Architecture Lens**: Implements `ADR-05` to bypass standard browser `EventSource` limitations (`GET` only, no custom auth headers), fully adhering to ReadHead-BE SSE specifications.
- **Acceptance Criteria**:
  - Connects to `POST /api/v1/ai/stream` with custom JSON payload and Bearer token.
  - Emits incremental text chunks smoothly with zero dropped or corrupted characters.
  - Clicking "Stop" aborts the network connection immediately while retaining partial text.
  - Client blocks attempts to trigger a 3rd concurrent stream.
- **Verification / Screenshot**:
  - Test: Vitest test verifying mock SSE chunk assembly with broken packet boundaries.
  - Screenshot: Side Panel displaying real-time streaming with animated cursor and active token count.

---

### [FE-05] Global State Architecture (Zustand) & Storage Abstraction Layer
- **Task ID & Feature Reference**: `TASK-FE-107`, `TASK-FE-108` | Feature B (`FR-CFG-01~07`), Feature D (`FR-CNT-05~07`), Feature J (`FR-SYN-01~08`)
- **Description**: Implement synchronous client-side state management using **Zustand** alongside a unified storage abstraction layer that harmonizes `chrome.storage.local`, `chrome.storage.session`, and standard browser `localStorage`/`sessionStorage` across Extension and Web contexts.
- **Plan & Codebase**:
  - `src/shared/services/storageAdapter.ts`: Unified async key-value interface with fallback detection (`isChromeExtensionEnvironment() ? chrome.storage.local : localStorage`).
  - `src/shared/stores/useAuthStore.ts`: Tracks `user`, `tokens`, `isAuthenticated`, `isLoading`, and `logout()`.
  - `src/shared/stores/useTabContextStore.ts`: Manages active `tabId`, `reading_session_id`, `page_url`, `page_title`, extracted article DOM snapshot, active text selection, and prompt draft.
  - `src/shared/stores/useSettingsStore.ts`: Manages reading preferences, AI provider defaults, accessibility profile (OpenDyslexic, bionic reading, line spacing), proactive assistance sensitivity, and domain blacklist. Supports guest mode fallback in local storage.
- **Scenarios & Edge Cases**:
  - *Guest Mode to Authenticated Transition*: User customizes dyslexia font settings as an unauthenticated guest; upon subsequent login, local settings are preserved and auto-merged into cloud profile.
  - *Storage Quota Exceeded*: Gracefully catches `QUOTA_BYTES` exceptions in `chrome.storage.local` with automatic eviction of stale article context caches.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Ensures user accessibility preferences persist across browser restarts, even before the user logs into an account.
  - 🛠️ **TechLead Lens**: Zustand's selector pattern (`useAuthStore(state => state.user)`) eliminates unnecessary component re-renders.
  - 🚀 **DevOps Lens**: Zero runtime dependencies on heavy external state sync servers; lightweight local serialization.
  - 🏛️ **Software Architecture Lens**: Enforces `ADR-04` by cleanly separating synchronous UI state (Zustand) from asynchronous server caching (TanStack Query).
- **Acceptance Criteria**:
  - Stores initialize instantly from persistent storage on extension boot.
  - State changes in settings immediately trigger reactive UI updates across all open views.
  - Storage adapter functions identically in Extension environment and Web Management Portal.
- **Verification / Screenshot**:
  - Test: `npm run test src/shared/stores/__tests__/stores.test.ts`
  - Screenshot: DevTools storage inspector showing normalized keys (`readhead_auth`, `readhead_settings`) stored cleanly.

---

## Phase 2: Chrome Extension Surfaces & In-Page Injection (Manifest V3)

### [FE-06] Background Service Worker, Chrome Lifecycle & Cross-Context Bus
- **Task ID & Feature Reference**: `TASK-FE-201` | Manifest V3 Constraints, `ADR-02`, `Rule 08`
- **Description**: Implement the central Background Service Worker (`background.ts`) acting as the message bus and lifecycle manager. Handles extension install/update events, tab activations, programmatic Content Script injection into pre-existing tabs, Chrome alarms for background sync, and opening the Side Panel.
- **Plan & Codebase**:
  - `src/extension/background/index.ts`: Service worker entry point.
  - `src/extension/background/messageBus.ts`: Typed message router handling `ExtensionMessage` events between Content Scripts, Side Panel, and Popup.
  - `src/extension/background/tabTracker.ts`: Tracks active tab changes (`chrome.tabs.onActivated`, `chrome.tabs.onUpdated`) and signals Side Panel to switch reading contexts.
  - Programmatic script injection: On `chrome.runtime.onInstalled`, queries all existing `http://*/*` and `https://*/*` tabs and executes `chrome.scripting.executeScript` to inject Content Scripts immediately, eliminating the need for users to manually refresh their open tabs.
  - Action click binding: `chrome.sidePanel.setPanelBehavior({ openPanelOnActionClick: true })`.
- **Scenarios & Edge Cases**:
  - *Service Worker Inactivity Termination*: MV3 service workers terminate after ~30 seconds of inactivity. Worker holds zero persistent in-memory variables; all cross-turn state is hydrated from `chrome.storage.local`.
  - *Restricted Browser Schemes*: Worker skips injection on `chrome://`, `chrome-extension://`, `edge://`, `about:blank` to prevent security permission exceptions.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Immediate usability upon installation without confusing "Please refresh your page to use ReadHead" error banners.
  - 🛠️ **TechLead Lens**: Exhaustive message discriminator typing (`type: 'EXTRACT_PAGE_CONTENT' | 'TAB_URL_CHANGED' | ...`) prevents unhandled runtime exceptions.
  - 🚀 **DevOps Lens**: Service worker bundle size kept strictly below 50KB with zero bloated external libraries to ensure instant wake-up execution.
  - 🏛️ **Software Architecture Lens**: Acts strictly as a stateless router; never performs heavy DOM parsing or runs long-lived streaming connections.
- **Acceptance Criteria**:
  - Fresh installation automatically injects Content Scripts into pre-opened browser tabs.
  - Clicking extension icon opens Chrome Side Panel directly.
  - Switching browser tabs broadcasts active tab information to Side Panel without memory leaks.
- **Verification / Screenshot**:
  - Command: Reload extension in `chrome://extensions` and check Service Worker console.
  - Screenshot: DevTools Service Worker console showing successful lifecycle initialization and clean message dispatch logs.

---

### [FE-07] Closed Shadow DOM Floating Action Menu & Selection Watcher
- **Task ID & Feature Reference**: `TASK-FE-202` | Feature D (`FR-CNT-01~04`), Feature E (`FR-AI-01`), `ADR-03`
- **Description**: Build the in-page text selection detector and floating quick-action bubble. Encapsulate the entire UI inside a **closed Shadow DOM** (`attachShadow({ mode: 'closed' })`) to guarantee absolute style isolation from host webpages. Employ **Event Capturing** to defeat aggressive host `stopPropagation()` blockers, and support full WCAG 2.1 AA keyboard navigation.
- **Plan & Codebase**:
  - `src/extension/content/SelectionWatcher.ts`: Detects text selection changes (`selectionchange`, `mouseup`, `keyup`).
  - `src/extension/content/FloatingActionMenu.tsx`: Floating action bubble offering instant action chips: *"Explain"*, *"Summarize"*, *"Translate"*, *"Ask AI"*.
  - `src/extension/content/shadowRoot.ts`: Creates custom element `<readhead-overlay-root>`, attaches closed Shadow DOM, and injects `shadow-dom.css`.
  - Event Capturing: Attach selection listeners with `{ capture: true }` on `window` and `document` to intercept selections even on platforms like Google Docs, Medium, and Substack that consume bubbling events.
  - Positioning & Collision Engine: Computes bounding rect of selection (`Range.getBoundingClientRect()`), positioning bubble 8px above selection, clamped within viewport boundaries.
  - Keyboard Hotkeys: Global shortcut (`Alt+R` / `Ctrl+Shift+E`) to invoke menu on selected text, `Tab` focus trapping within bubble, and `Escape` to dismiss.
- **Scenarios & Edge Cases**:
  - *Host Event Cancellation*: Websites invoking `e.stopPropagation()` in bubble phase fail to stop our captured listener; selection is registered reliably.
  - *Micro-Selections / Trailing Whitespace*: Highlights < 3 characters or pure whitespace are filtered out, preventing annoying phantom popups.
  - *Screen Edge Collisions*: Selections at the very top or bottom of the viewport flip the popup vertically to stay completely visible.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Delightful, friction-free reader experience—highlighting text immediately surfaces contextual assistance without forcing the user to leave their reading flow.
  - 🛠️ **TechLead Lens**: Closed Shadow DOM ensures zero CSS pollution to or from host websites; proper listener teardown on window blur avoids zombie handlers.
  - 🚀 **DevOps Lens**: Minimal inline CSS footprint with pre-compiled Tailwind utilities injected directly into the shadow root.
  - 🏛️ **Software Architecture Lens**: Decoupled content script UI communicates with the Side Panel exclusively via background message dispatch, preserving strict separation between host DOM and extension state.
- **Acceptance Criteria**:
  - Selecting text on any website displays the floating action bubble within 50ms.
  - Clicking *"Explain"* opens the Side Panel and initiates the explanation stream with the selected text.
  - Pressing `Escape` instantly dismisses the menu and restores original focus.
  - Floating menu styling remains 100% immune to host page CSS resets.
- **Verification / Screenshot**:
  - Test: Manual selection on complex websites (e.g. Medium, GitHub, Wikipedia, NYTimes).
  - Screenshot: In-page screenshot showing the floating bubble rendered cleanly over selected text on a heavily styled webpage.

---

### [FE-08] In-Page Article Extractor, SPA Interceptor & PDF Text Ingestion
- **Task ID & Feature Reference**: `TASK-FE-203` | Feature D (`FR-CNT-01~07`), `ADR-06`, `ADR-11`, `ISSUE-001`
- **Description**: Implement clean web article text and metadata extraction using `@mozilla/readability`. Support modern Single Page Applications (SPAs) by intercepting the HTML5 History API, enforce tiered fallback for paywalls/protected DOMs, and provide local zero-binary PDF text extraction via bundled `pdf.js`.
- **Plan & Codebase**:
  - `src/extension/content/DOMExtractor.ts`:
    - Clones DOM and feeds into `@mozilla/readability` to extract `title`, `byline`, `content`, `textContent`, `excerpt`.
    - Tiered Fallback Engine:
      1. Primary: `@mozilla/readability` parses `<article>` / `<main>`.
      2. Fallback 1: High-density text node container scanning (`div`, `section`).
      3. Fallback 2: If extracted text < 150 characters (gated / paywall), display user guidance banner: *"Protected page. Highlight specific text to ask questions."*
  - `src/extension/content/spaInterceptor.ts`: Hooks `history.pushState`, `history.replaceState`, `popstate`, and observes `MutationObserver` on `document.title` and `location.href`. Informs background worker of URL path changes to reset stale conversation context.
  - `src/extension/content/pdfExtractor.ts`: Client-side plain text extractor for browser-rendered PDF documents using bundled `pdfjs-dist`. Extracts plain text up to max 100KB, strictly rejecting raw binary uploads (`ADR-011`).
  - Cross-Origin Iframe Guard: Checks `window !== window.top`. Bypasses inaccessible sandboxed frames and displays selective guidance.
- **Scenarios & Edge Cases**:
  - *Dynamic Client Navigation*: User transitions from Article A to Article B on Substack/Twitter without page reload. Extractor detects URL change, purges old cache, extracts new text, and updates `useTabContextStore`.
  - *Heavy Paywalled Sites*: Bloomberg/WSJ obfuscated content yields empty readability result. Fallback 2 triggers, allowing user to manually highlight visible paragraphs without crashing.
  - *PDF Ingestion*: When viewing a local or web PDF (`application/pdf`), `pdfExtractor` extracts page text sequentially, truncates at 100KB with a warning chip, and passes clean text to Side Panel.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Reliable article understanding across 99% of web content, including complex SPAs and PDFs, eliminating cryptic empty response errors.
  - 🛠️ **TechLead Lens**: Cloning DOM before passing to Readability avoids destructive mutations to host live elements.
  - 🚀 **DevOps Lens**: Zero server-side headless browser rendering costs; 100% of DOM extraction and PDF parsing occurs locally on the client machine.
  - 🏛️ **Software Architecture Lens**: Upholds `ADR-011` (Zero Binary Uploads) by converting PDFs to plain text client-side, guaranteeing lightweight backend payloads.
- **Acceptance Criteria**:
  - Readability extracts clean text, stripping navigation menus, ads, and footers.
  - SPA navigation triggers automatic context re-extraction within 250ms of URL change.
  - PDFs render text preview and transmit clean text to `/api/v1/ai/stream`.
  - Total extracted payload never exceeds 100KB.
- **Verification / Screenshot**:
  - Test: `npm run test src/extension/content/__tests__/DOMExtractor.test.ts`
  - Screenshot: Console output and Side Panel banner showing extracted article title, word count, and reading time.

---

### [FE-09] Non-Destructive In-Page Accessibility Modifiers (OpenDyslexic & Bionic)
- **Task ID & Feature Reference**: `TASK-FE-204` | Feature B (`FR-CFG-01~07`), `ADR-06`, `ADR-10`, `NFR-ACC-01~04`
- **Description**: Provide in-page typography and reading flow enhancements directly on the host website: OpenDyslexic font injection and Bionic Reading fixation bolding. Modifications must be 100% non-destructive and easily reversible, without breaking host React/Vue Virtual DOM nodes or causing layout shifts.
- **Plan & Codebase**:
  - `src/extension/content/AccessibilityRenderer.ts`: Core accessibility injection engine.
  - OpenDyslexic Injection:
    - Bundles `.woff2` font assets inside extension package.
    - References secure local URL scheme: `chrome-extension://<EXTENSION_ID>/assets/fonts/OpenDyslexic-Regular.woff2`.
    - Injects custom stylesheet targeting article content paragraphs with enhanced `letter-spacing: 0.05em` and `line-height: 1.6`.
  - Bionic Reading Engine:
    - Traverses strictly text nodes (`Node.TEXT_NODE`) within content tags (`<article>`, `<p>`).
    - Explicitly skips interactive/executable elements: `<a>`, `<button>`, `<input>`, `<textarea>`, `<code>`, `<pre>`, `<script>`, `<style>`.
    - Bolds the first 30%-50% of each word (e.g. `<b>Int</b>elligent <b>Read</b>ing`).
    - Uses `WeakMap<Node, string>` to store original text snapshots for instantaneous 1-click restoration without page reload.
- **Scenarios & Edge Cases**:
  - *React/Vue Virtual DOM Crash*: Host Single Page Applications throw fatal `removeChild` errors if an extension replaces DOM nodes managed by their reconciliation engine. Targeting text nodes without altering element tree topology prevents VDOM collisions.
  - *Host CSP Blocking Remote Fonts*: Strict `font-src` rules block Google Fonts; local extension package URLs declared in `web_accessible_resources` bypass CSP reliably.
  - *Toggle Off State*: User unchecks Bionic Reading in popup; `WeakMap` restore runs in < 15ms, reverting all text nodes to their pristine state.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Direct fulfillment of core accessibility business requirements (`BG-06`, `BR-11`), enabling dyslexic and neurodivergent readers to comprehend text significantly faster.
  - 🛠️ **TechLead Lens**: `WeakMap` cache prevents memory leaks across long browsing sessions and ensures O(N) linear-time restoration.
  - 🚀 **DevOps Lens**: Font assets are compressed as WOFF2 (< 120KB) to ensure minimal extension bundle footprint.
  - 🏛️ **Software Architecture Lens**: Strictly isolated from extension business logic, operating purely as a presentation adapter over the host DOM.
- **Acceptance Criteria**:
  - Toggling OpenDyslexic renders the font cleanly on article body text across any website.
  - Bionic Reading bolds words accurately while preserving code blocks and hyperlinks intact.
  - Disabling the features immediately restores pristine DOM with zero remaining artifacts or layout jitter.
- **Verification / Screenshot**:
  - Test: Visual regression check on standard article layouts.
  - Screenshot: Side-by-side comparison of a Wikipedia article before and after applying OpenDyslexic and Bionic Reading.

---

### [FE-10] Local-First Dwell Time Tracker & Proactive Reading Cues Engine
- **Task ID & Feature Reference**: `TASK-FE-205` | Feature G (`FR-PRV-01~06`), `ADR-06`, `BR-10`
- **Description**: Implement a local-first user reading pace and dwell time monitor that detects when a reader is struggling with dense, complex text. Triggers subtle, non-intrusive in-page assistance chips. Includes an inactivity guard, minimum paragraph threshold, dismissal fatigue snooze policy, and reliable beacon flush.
- **Plan & Codebase**:
  - `src/extension/content/DwellTracker.ts`: In-memory paragraph reading pace monitor.
  - `src/extension/content/ProactiveHintChip.tsx`: Floating chip anchored to the struggling paragraph with actions: *"Explain this simply"* or *"Dismiss"*.
  - Inactivity Guard: Listens for mouse movement, scrolling, and keydown. If zero user activity occurs for > 45 seconds or `document.hidden = true`, timer pauses immediately, avoiding false positives when the user steps away.
  - Content Filter: Tracks only paragraphs (`<p>`) containing at least 25 words or 150 characters, ignoring headers, footers, and sidebars.
  - Dismissal Fatigue Policy: If a user closes a hint chip, activates a 5-minute domain cooldown and suppresses hints for the next 3 consecutive paragraphs.
  - Offline Silence: Suppresses proactive triggers when `navigator.onLine = false`.
  - Backend Assistance: If paragraph exceeds difficulty threshold, calls `POST /api/v1/ai/proactive-suggest` to fetch a 1-sentence plain-language cue.
- **Scenarios & Edge Cases**:
  - *User Steps Away from Desk*: User leaves browser open on a paragraph for 10 minutes. Timer halts at 45s; no annoying proactive popup is displayed upon return.
  - *Annoyed User Dismisses Chip*: User clicks "X" on a hint. System respects user choice, snoozing all hints across the domain for 5 minutes.
  - *Privacy Mode Active*: If `privacy_mode_enabled: true`, proactive tracking runs 100% locally with zero telemetry emitted.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Delivers the signature "intelligent assistant" value proposition without ever feeling spammy, annoying, or intrusive.
  - 🛠️ **TechLead Lens**: Lightweight debounced scroll listener (`requestAnimationFrame`) ensures zero frame drops or scrolling lag (maintains 60 FPS).
  - 🚀 **DevOps Lens**: Heuristic filtering prevents excessive API calls to `/proactive-suggest`, keeping backend inference costs near zero.
  - 🏛️ **Software Architecture Lens**: Follows `ADR-06` (Local-First Heuristic), computing user attention metrics entirely on the client without streaming sensitive browsing activity.
- **Acceptance Criteria**:
  - Inactivity > 45s pauses dwell timer immediately.
  - Paragraphs < 25 words are completely ignored.
  - Hint chip appears smoothly at the bottom-right of the viewport with a non-intrusive soundless animation.
  - Dismissing a hint silences suggestions for 5 minutes.
- **Verification / Screenshot**:
  - Test: `npm run test src/extension/content/__tests__/DwellTracker.test.ts`
  - Screenshot: Host webpage showing a subtle floating hint chip: *"Need help with this technical passage? [Simplify]"*.

---

## Phase 3: Chrome Side Panel, Chat Interaction & Comprehension Quizzes

### [FE-11] Chrome Side Panel Workspace, Tab Isolation & AI Model Switcher
- **Task ID & Feature Reference**: `TASK-FE-206a` | Feature D (`FR-CNT-05~07`), Feature E (`FR-AI-01~08`), `ADR-12`
- **Description**: Build the primary Chrome Side Panel application workspace (`sidepanel.tsx`). Implements strict tab isolation via client-generated `reading_session_id`, dynamic context switching upon tab activation, model switcher (`Gemini`, `OpenAI`, `Claude`), and clean chat timeline re-keying.
- **Plan & Codebase**:
  - `src/extension/sidepanel/SidePanelApp.tsx`: Main side panel workspace container.
  - `src/extension/sidepanel/components/HeaderBar.tsx`: Active domain badge, connection status, clear conversation button, and model dropdown.
  - `src/extension/sidepanel/components/ModelSelector.tsx`: Dropdown selector mapping available models based on active user BYOK keys (`gemini-1.5-flash`, `gpt-4o-mini`, `claude-3-5-sonnet`).
  - Tab Isolation Coordinator:
    - Generates unique `reading_session_id` stored in tab's `sessionStorage`.
    - Subscribes to `chrome.tabs.onActivated`: When user switches browser tabs, Side Panel smoothly transitions with a skeleton loader, loads the new tab's active session, and displays the corresponding chat history.
    - If new tab has no existing history, displays clean welcome view with quick-start prompt pills (*"Summarize article"*, *"Extract key takeaways"*).
- **Scenarios & Edge Cases**:
  - *Rapid Tab Switching*: User rapidly clicks through 5 different browser tabs. Race conditions are handled by an active tab requestId guard, preventing stale tab context from overwriting the view.
  - *No Stored API Key*: If user selects Claude but has not provided an Anthropic key, displays a direct warning badge: *"No active Claude key found. Configure in Settings [Open]"*.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Keeps user reading conversations neatly organized per webpage, preventing embarrassing cross-tab prompt confusion.
  - 🛠️ **TechLead Lens**: Strict React keying (`key={reading_session_id}`) guarantees state teardown and eliminates visual ghosting between tabs.
  - 🚀 **DevOps Lens**: Side panel loads in < 200ms by utilizing localized bundle chunks and cached session data.
  - 🏛️ **Software Architecture Lens**: Enforces `ADR-12` (Tab-Isolated Conversations) on the frontend, ensuring every payload transmitted to BE contains valid `reading_session_id` and `tab_id`.
- **Acceptance Criteria**:
  - Side panel opens alongside webpage without covering or resizing the host page layout abnormally.
  - Switching browser tabs updates the Side Panel conversation history within 100ms.
  - Switching AI model updates the active provider state for subsequent prompts immediately.
- **Verification / Screenshot**:
  - Manual test: Open Tab 1 (TechCrunch) and Tab 2 (Wikipedia). Chat in Tab 1, switch to Tab 2, verify chat is isolated.
  - Screenshot: Side Panel showing domain header, model dropdown selector, and isolated tab timeline.

---

### [FE-12] Real-Time Streaming Chat, KaTeX Math & Interactive Citations
- **Task ID & Feature Reference**: `TASK-FE-206b` | Feature E (`FR-AI-01~12`), `BR-12~14`, `NFR-REL-01~05`
- **Description**: Implement the real-time AI conversation feed in the Side Panel. Features live token streaming with typing animation, GitHub-Flavored Markdown rendering, LaTeX math notation via KaTeX, code syntax highlighting with copy buttons, and interactive source grounding citations that auto-scroll the host webpage.
- **Plan & Codebase**:
  - `src/extension/sidepanel/components/MessageList.tsx`: Virtualized chat timeline rendering user and assistant turns.
  - `src/extension/sidepanel/components/ChatMessageItem.tsx`: Individual bubble handling markdown, timestamps, and model badges.
  - `src/extension/sidepanel/components/MarkdownViewer.tsx`: Custom `react-markdown` setup integrated with `remark-gfm` (tables, strikethrough), `rehype-katex` (math equations), and Prism code highlighting.
  - `src/extension/sidepanel/components/CitationPill.tsx`: Clickable grounding badge (e.g. `[Source: §3.2]`). Clicking sends message to Content Script to highlight and smoothly scroll (`scrollIntoView({ behavior: 'smooth', block: 'center' })`) the host webpage to the exact cited paragraph.
  - `src/extension/sidepanel/components/PromptInputBox.tsx`: Auto-resizing textarea with character count, "Ask about page" pill, and submit button.
- **Scenarios & Edge Cases**:
  - *LaTeX Math Expressions*: Academic physics/math papers stream formulas like `$E = mc^2$` or display math `$$\int_0^\infty e^{-x^2} dx$$`. Rendered crisply via KaTeX without raw formatting leakage.
  - *Auto-Scroll on Long Stream*: Viewport auto-scrolls down as tokens stream in; if user manually scrolls up to read earlier text, auto-scroll pauses to prevent annoying scroll jumping.
  - *Uncertainty Declarations*: When model response contains uncertainty markers (`[UNCERTAIN]`), renders a subtle amber confidence badge informing the user to verify against source text (`BR-13`).
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Superior reading assistance—citations bridge the AI response directly to the source text on the webpage, eliminating hallucinations and building trust.
  - 🛠️ **TechLead Lens**: Memoized markdown component prevents heavy re-parsing of static historical messages during active streaming.
  - 🚀 **DevOps Lens**: KaTeX and Prism styles are scoped and bundled locally, requiring zero external CDN requests.
  - 🏛️ **Software Architecture Lens**: Decouples presentation from streaming logic; `useAIStream` supplies raw tokens while `MarkdownViewer` handles progressive DOM rendering.
- **Acceptance Criteria**:
  - Tokens stream into the message feed smoothly without UI stuttering.
  - Code blocks render syntax colors with a functional "Copy Code" button.
  - Clicking a citation pill scrolls the host webpage directly to the referenced sentence with a temporary highlight glow.
  - KaTeX mathematical expressions format correctly.
- **Verification / Screenshot**:
  - Test: Stream a sample markdown response containing a code block, a math formula, and a citation pill.
  - Screenshot: Side Panel displaying formatted message with syntax highlighted code and math formula.

---

### [FE-13] Interactive Comprehension Quiz Card Subsystem & Evaluation Review
- **Task ID & Feature Reference**: `TASK-FE-207` | Feature F (`FR-CMP-01~05`), `BG-03`, `BR-06~07`
- **Description**: Build the interactive comprehension quiz subsystem in the Side Panel. Automatically renders 3-5 multiple-choice questions generated from the active article content, manages radio selection state, submits answers to `/api/v1/quiz/{id}/submit`, and displays immediate grading with grounded rationales.
- **Plan & Codebase**:
  - `src/extension/sidepanel/components/QuizCard.tsx`: Master quiz card container with progress bar (e.g. "Question 2 of 4").
  - `src/extension/sidepanel/components/QuizQuestionItem.tsx`: Question text with 4 styled radio options.
  - `src/extension/sidepanel/components/QuizReviewView.tsx`: Displays post-submission score (e.g. "4/5 - 80%"), color-coded review (green for correct, red for incorrect), and in-depth explanation grounded in the article.
  - Trigger Integration: Quick-action button in Side Panel header *"Test Comprehension"* and in-page completion prompt when reaching the bottom of a long article.
- **Scenarios & Edge Cases**:
  - *Short / Ineligible Articles*: User clicks quiz on a page with < 300 words. Frontend catches condition and shows helpful banner: *"Article is too short to generate a meaningful quiz. Please try a longer article."*
  - *Accidental Re-submission*: Once submitted, radio options lock immediately into disabled state; prevents duplicate submission errors (`400 QUIZ_ALREADY_SUBMITTED`).
  - *Partial Answer Submission*: User attempts to submit with unanswered questions; displays validation indicator highlighting skipped questions.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Directly supports learning retention and active reading goals, providing tangible educational value for students and researchers.
  - 🛠️ **TechLead Lens**: Clean state machine (`idle -> generating -> active -> submitting -> graded`) prevents race conditions and invalid transitions.
  - 🚀 **DevOps Lens**: Quiz state cached in `chrome.storage.session` so accidental panel closes do not lose active quiz progress.
  - 🏛️ **Software Architecture Lens**: Strictly respects backend contract (`POST /quiz/generate` returns questions without answers; grading is performed authoritatively on backend).
- **Acceptance Criteria**:
  - Generates 3-5 questions from current page text with interactive option selection.
  - Submitting returns score, highlighting correct/incorrect options.
  - Detailed explanation appears for each question after grading.
  - Radio options are disabled post-submission to prevent re-taking.
- **Verification / Screenshot**:
  - Test: `npm run test src/extension/sidepanel/__tests__/QuizCard.test.ts`
  - Screenshot: Side Panel showing graded quiz card with green/red feedback badges and grounded rationale.

---

### [FE-14] AI Response Feedback & Accuracy Reporting Widget
- **Task ID & Feature Reference**: `TASK-FE-206c` | Feature E (`BR-15`, `BG-08`)
- **Description**: Implement the user satisfaction and accuracy reporting widget attached to every AI assistant turn. Allows users to rate responses with Thumbs Up or Thumbs Down, specify problem categories (hallucination, inaccurate, formatting), provide optional comments, and seamlessly toggle ratings.
- **Plan & Codebase**:
  - `src/extension/sidepanel/components/FeedbackWidget.tsx`: Thumbs Up / Thumbs Down button group.
  - `src/extension/sidepanel/components/FeedbackReportPopover.tsx`: Popover modal triggered on Thumbs Down offering 4 categorical chips:
    - `inaccurate`: Factual inaccuracy compared to article.
    - `hallucination`: Model cited information not present in the source text.
    - `harmful`: Inappropriate or offensive content.
    - `other`: Formatting or verbosity problem.
    - Optional textarea for specific feedback.
  - Service Integration: Calls `POST /api/v1/ai/feedback` with payload `{ message_id, rating, report_reason, comment }`.
  - Seamless Toggle: Re-clicking Thumbs Up atomically changes rating from unhelpful to helpful and dismisses reporting details.
- **Scenarios & Edge Cases**:
  - *Rapid Multi-Click*: User clicks thumbs up 5 times in a second; debounced API call ensures only 1 request reaches the backend.
  - *Network Failure on Feedback*: If offline, feedback is queued in client outbox and submitted once reconnected.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Collects crucial human-in-the-loop accuracy data to benchmark AI quality and track prompt engineering regressions.
  - 🛠️ **TechLead Lens**: Optimistic UI update provides immediate visual confirmation while network call completes asynchronously.
  - 🚀 **DevOps Lens**: Zero impact on streaming performance; runs completely out-of-band after message completion.
  - 🏛️ **Software Architecture Lens**: Links feedback strictly to backend `message_id`, preserving full relational integrity for analytics.
- **Acceptance Criteria**:
  - Clicking Thumbs Up updates icon color instantly and submits rating.
  - Clicking Thumbs Down opens category popover; submitting transmits categorization to backend.
  - Toggling rating updates existing record on server without creating duplicates.
- **Verification / Screenshot**:
  - Test: Component test verifying optimistic UI toggle and API payload generation.
  - Screenshot: Side Panel showing active feedback popover with categorization chips selected.

---

### [FE-15] Action Toolbar Popup & Options Page (BYOK Vault & Settings)
- **Task ID & Feature Reference**: `TASK-FE-208`, `TASK-FE-209` | Feature B, Feature C (`FR-KEY-01~07`), `ADR-03`
- **Description**: Build the extension Toolbar Popup (`popup.tsx`) for fast status inspection and master toggles, along with the full Options Page (`options.tsx`) for BYOK key vault management (Google Gemini, OpenAI, Claude), domain blacklisting, and accessibility profile customization.
- **Plan & Codebase**:
  - `src/extension/popup/PopupApp.tsx`:
    - Connection indicator (Online / Offline / Auth status).
    - Active AI provider badge.
    - Master toggle switches: *Extension Active*, *Accessibility Mode*, *Proactive Hints*.
    - Prominent button: *"Open Side Panel"*.
  - `src/extension/options/OptionsApp.tsx`:
    - Full-page settings tabs: *API Keys (BYOK)*, *Reading Preferences*, *Privacy & Blacklist*, *Account*.
    - Key Vault Manager: Input boxes for Gemini, OpenAI, Anthropic keys with input masking (`••••••••••••sk-4aF9`), "Test Key" button firing `POST /api-keys/{provider}/validate`, and "Delete Key" trigger.
    - Domain Blacklist Manager: Tag input allowing users to add domains (e.g. `chase.com`, `internal-wiki.corp`) to completely disable extension operations on sensitive sites.
- **Scenarios & Edge Cases**:
  - *Invalid Key Entered*: User inputs malformed OpenAI key. "Test Key" button displays inline error: *"Key validation failed: 401 Unauthorized from OpenAI"*. Key is not activated.
  - *Secret Masking Guard*: Plaintext keys are NEVER displayed after initial input. The UI only displays the masked hint returned by BE (`sk-...4aF9`).
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Clear, transparent key management empowers users to bring their own AI quotas without feeling locked into a subscription.
  - 🛠️ **TechLead Lens**: Input forms powered by `react-hook-form` + `zod` prevent invalid formats before touching the network.
  - 🚀 **DevOps Lens**: Options page functions as an isolated HTML entry point, minimizing resource consumption during standard reading.
  - 🏛️ **Software Architecture Lens**: Direct compliance with `Rule 09` (Zero-Knowledge BYOK Vault): frontend never writes raw keys to `localStorage` or browser console.
- **Acceptance Criteria**:
  - Popup opens within 100ms of clicking toolbar icon.
  - Options page allows adding, testing, and deleting BYOK keys.
  - Testing an API key displays a live green success or red failure indicator.
  - Adding a blacklisted domain immediately disables extension functionality on that host.
- **Verification / Screenshot**:
  - Command: Open Chrome extension options via right-click icon -> Options.
  - Screenshot: Options page showing BYOK key vault table with masked keys and validation status badges.

---

## Phase 4: Privacy Guardrails, Offline Resilience & Multi-Device Sync

### [FE-16] Client-Side Pre-Flight Sensitive Content Inspector & Domain Privacy Gate
- **Task ID & Feature Reference**: `TASK-FE-301`, `TASK-FE-303`, `TASK-FE-304` | Feature H (`FR-SEC-01~12`), `ADR-06`, `Rule 09`
- **Description**: Build the client-side pre-flight sensitive content scanner and domain privacy firewall. Inspects 100% of outbound text (selected text, article excerpts, user prompts) for credentials, API keys, passwords, credit cards, and PII before any network transmission occurs. Displays a default-block modal with "Edit" or "Override", and enforces domain blacklists.
- **Plan & Codebase**:
  - `src/shared/services/sensitiveDataInspector.ts`:
    - Regex & heuristic detector:
      - API keys (`sk-[A-Za-z0-9]{20,}`, `AIza[0-9A-Za-z-_]{35}`, GitHub tokens `ghp_...`).
      - Passwords & private keys (`-----BEGIN PRIVATE KEY-----`).
      - Financial numbers (Credit cards via Luhn algorithm validation).
      - Government identifiers (SSN, national IDs).
    - Returns `{ is_sensitive: boolean, matches: Array<{ type, snippet, redacted }> }`.
  - `src/shared/components/SensitiveDataModal.tsx`:
    - Default-block warning dialog explaining that sensitive information was detected.
    - Shows redacted preview of offending lines.
    - Action buttons:
      - *"Edit & Remove"* (focuses input cursor at the sensitive snippet).
      - *"Override & Send Anyway"* (sets `bypass_protection: true` with logged user consent).
      - *"Cancel"*.
  - `src/extension/background/domainGate.ts`: Background filter checking active tab URL against `user_settings.blacklisted_domains`. If matched, completely disables extension injection and UI chips on that origin.
- **Scenarios & Edge Cases**:
  - *Pasting Code with Credentials*: User pastes a config snippet containing `AWS_SECRET_ACCESS_KEY`. Pre-flight scanner intercepts transmission before socket open, halts streaming request, and opens warning modal.
  - *Internal Corporate Domains*: User accesses `https://intranet.company.internal` which is in their blacklist. Extension icon grays out; Content Script is completely inert.
  - *User Chooses to Edit*: Clicking "Edit" does not discard prompt; it returns the user to the prompt box with offending tokens highlighted for easy deletion.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Vital data protection guarantee (`FR-SEC-04`, `FR-SEC-07`), eliminating user fear of accidental enterprise credential leaks to AI models.
  - 🛠️ **TechLead Lens**: High-performance compiled regex matching runs in < 5ms, avoiding any noticeable typing or submission lag.
  - 🚀 **DevOps Lens**: Halting sensitive data locally prevents enterprise compliance violations, proxy security alerts, and unnecessary backend processing.
  - 🏛️ **Software Architecture Lens**: Adheres to defense-in-depth: client performs pre-flight inspection before backend re-validates at `/api/v1/ai/check-sensitive`.
- **Acceptance Criteria**:
  - Prompt containing test API key pattern is blocked before firing any HTTP or SSE request.
  - Modal provides clear options to edit or override with explicit consent.
  - Blacklisted domains show disabled extension badge with zero DOM injection.
- **Verification / Screenshot**:
  - Test: `npm run test src/shared/services/__tests__/sensitiveDataInspector.test.ts`
  - Screenshot: Warning modal displaying detected credential pattern with "Edit" and "Override & Send" buttons.

---

### [FE-17] Continuous Prompt Draft Autosave, Offline Resilience & Session Purge
- **Task ID & Feature Reference**: `TASK-FE-302`, `TASK-FE-305` | Feature A (`FR-ACC-04`), Feature I, `FR-ERR-04`, `ISSUE-001`
- **Description**: Build an offline resilience engine and draft preservation system. Implements continuous debounced autosave of prompt drafts to `chrome.storage.local` keyed by tab session, automatic draft restoration upon browser restart or tab switch, strict Anti-Auto-Retry compliance upon reconnect, and an instantaneous multi-account logout purge protocol.
- **Plan & Codebase**:
  - `src/shared/services/draftRecoveryService.ts`:
    - Debounced (300ms) continuous autosave of user input keyed by `draft_${hash(page_url)}_${reading_session_id}`.
    - Restores draft into Side Panel prompt box upon tab activation, closed tab restore (`Ctrl+Shift+T`), or browser relaunch.
  - `src/shared/services/networkMonitor.ts`:
    - Listens to `window.online` and `window.offline` events.
    - Anti-Auto-Retry Guard: When network reconnects, displays a green toast: *"Internet connection restored"*. **NEVER** automatically re-dispatches pending prompts, preventing duplicate billing or unintended LLM queries (`FR-ERR-04`).
  - `src/shared/services/sessionPurgeService.ts`:
    - Master logout handler: Clears tokens, drafts, cached article extracts, and IndexedDB outbox.
    - Broadcasts `ACTION_PURGE_SESSION` across all extension surfaces (Side Panel, Popup, Content Scripts).
    - Aborts all active in-flight fetch calls via `AbortController`.
- **Scenarios & Edge Cases**:
  - *Sudden Browser Crash*: User writes a 500-word analysis prompt; laptop battery dies. Upon restarting Chrome and opening ReadHead, the exact drafted prompt is restored in the textarea.
  - *Multi-Account Laptop Handover*: User A logs out; User B logs in. Session purge ensures User B cannot see User A's un-sent drafts, cached articles, or conversation history (`FR-ACC-04`).
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Protects users against frustrating data loss during network hiccups, and guarantees complete privacy between shared family/work computer accounts.
  - 🛠️ **TechLead Lens**: Debounced storage writes eliminate disk thrashing; hashing URL keys ensures drafts never leak across unrelated pages.
  - 🚀 **DevOps Lens**: Zero server storage needed for ephemeral in-progress drafts; 100% managed locally.
  - 🏛️ **Software Architecture Lens**: Upholds `FR-ERR-04` by requiring explicit user interaction to send prompts after reconnecting.
- **Acceptance Criteria**:
  - Typing in prompt box persists text to storage within 300ms.
  - Reloading tab or opening closed tab restores un-sent draft completely.
  - Network reconnection displays toast without auto-submitting pending prompt.
  - Logging out wipes all local drafts, caches, and resets stores instantly across all tabs.
- **Verification / Screenshot**:
  - Test: Type draft, terminate extension background worker, reload, verify draft is present.
  - Screenshot: Side Panel displaying restored draft with timestamp indicator.

---

### [FE-18] Multi-Device Incremental Sync Client, Conflict Resolution & Offline Outbox
- **Task ID & Feature Reference**: `TASK-FE-306`, `TASK-FE-307` | Feature J (`FR-SYN-01~08`), `ADR-07`, `ADR-13`
- **Description**: Implement the multi-device incremental synchronization client. Utilizes a Base64 Composite Cursor `(updated_at, id)` to pull cloud mutations without skipping records, queues offline actions in an IndexedDB outbox (`sync_outbox` max 500 items), and surfaces a visual visual-diff conflict resolution dialog upon encountering server `409 SYNC_CONFLICT`.
- **Plan & Codebase**:
  - `src/shared/services/syncClient.ts`:
    - `pullChanges()`: Calls `POST /api/v1/sync/pull` with composite cursor, paging in batches of 100 items. Updates local stores with new conversations, highlights, and settings.
    - `pushMutations()`: Flushes local mutations queue via `POST /api/v1/sync/push`.
  - `src/shared/services/outboxQueue.ts`:
    - IndexedDB storage for offline mutations (e.g. created highlight, updated setting, deleted conversation).
    - Capped at 500 items (FIFO eviction with warning at 90% capacity).
    - Auto-flushes upon network online event in batches of 100.
  - `src/shared/components/ConflictResolverModal.tsx`:
    - Intercepts `409 SYNC_CONFLICT` containing `server_record` and `client_version`.
    - Renders side-by-side visual diff modal allowing user to choose:
      - *"Keep Local Changes"* (forces override with bumped version).
      - *"Accept Cloud Version"* (overwrites local copy).
- **Scenarios & Edge Cases**:
  - *Long Offline Flight*: User highlights 30 articles and edits notes while on an airplane. Upon landing and reconnecting, outbox flushes all 30 operations in sequential order to the backend without data loss.
  - *Simultaneous Edit on Laptop & Desktop*: User updates accessibility profile on Laptop while changing it on Desktop. Backend returns 409 Conflict. Conflict modal appears, visually highlighting differing properties and prompting user decision.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Seamless multi-device reading experience—notes taken on office desktop appear instantly on home laptop.
  - 🛠️ **TechLead Lens**: Optimistic version locking and Composite Cursor `(updated_at, id)` guarantee mathematical consistency and eliminate missed records.
  - 🚀 **DevOps Lens**: Chunked syncing (batches of 100) prevents API gateway timeouts and DB connection spikes.
  - 🏛️ **Software Architecture Lens**: Concrete realization of `ADR-07` and `ADR-13`, ensuring offline-first resilience with zero silent data overwrites.
- **Acceptance Criteria**:
  - Offline operations queue reliably in IndexedDB and flush automatically when back online.
  - Pull sync accurately applies remote changes using composite cursor.
  - `409 Conflict` pauses sync queue and displays side-by-side diff resolution dialog.
  - Selected resolution writes cleanly to server without subsequent errors.
- **Verification / Screenshot**:
  - Test: `npm run test src/shared/services/__tests__/syncClient.test.ts`
  - Screenshot: Side-by-side Conflict Resolver Modal displaying local vs cloud differences with "Keep Local" / "Accept Cloud" buttons.

---

### [FE-19] In-Page Web Highlights Manager & Dynamic SPA Re-Hydration
- **Task ID & Feature Reference**: `TASK-FE-203b` | Feature I (`FR-HIS-01~06`, `BR-18~21`)
- **Description**: Build the in-page text highlighting and annotation system. Enables users to highlight webpage passages with custom colors (yellow, green, blue, pink) and attach personal notes. Features dynamic re-hydration via container `MutationObserver` to re-apply saved highlights on client-rendered SPA routes, and supports visual de-highlighting upon deletion.
- **Plan & Codebase**:
  - `src/extension/content/HighlightManager.ts`:
    - Wraps selected text ranges in custom `<mark class="readhead-highlight" data-highlight-id="...">` tags.
    - Stores text offset anchors (`startContainer`, `startOffset`, `endContainer`, `endOffset`, text snippet hash) for reliable re-attachment.
  - `src/extension/content/HighlightPopover.tsx`: Popover attached to `<mark>` showing color picker, attached note, and delete button.
  - Dynamic Re-Hydration Observer: Observes page DOM mutations with a 300ms debounce. When an SPA re-renders or navigates, searches for anchor text snippets and re-injects highlight markers dynamically without requiring a page reload.
  - Visual Deletion & Tombstone: Clicking delete removes `<mark>` tag, restores pristine text node, and dispatches soft-delete to `DELETE /api/v1/reading/highlights/{id}`.
- **Scenarios & Edge Cases**:
  - *Dynamic DOM Shifts (Ads / Lazy Loading)*: Late-loading images or dynamic banners shift text coordinates; anchor matching combines character offsets with surrounding context string hashing to guarantee accurate placement.
  - *Overlapping Highlights*: Attempting to highlight text that already contains a highlight splits and merges boundary tags gracefully without malforming the HTML tree.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Transforms transient web reading into a persistent, personalized study workspace.
  - 🛠️ **TechLead Lens**: Context hashing fallback ensures highlights re-attach even if host page minorly alters surrounding whitespace.
  - 🚀 **DevOps Lens**: Highlights sync seamlessly through standard `/reading/highlights` REST endpoints and sync outbox.
  - 🏛️ **Software Architecture Lens**: Integrates with Feature J sync engine; deletions generate local tombstones ensuring highlights deleted on mobile vanish from desktop.
- **Acceptance Criteria**:
  - Selecting text and choosing a color applies highlight styling immediately.
  - Refreshing or returning to the page re-hydrates highlights in their correct visual positions.
  - Clicking a highlight reveals the note popover.
  - Deleting a highlight cleanly un-wraps the `<mark>` element without altering text content.
- **Verification / Screenshot**:
  - Test: Create highlights across multiple paragraphs, navigate away and back, verify presence.
  - Screenshot: Webpage showing highlighted text passages in multiple colors with an active note popover.

---

## Phase 5: Web Management Portal (User Dashboard, Admin Telemetry & Documentation)

### [FE-20] Web Portal Application Scaffolding & User Reading Analytics Dashboard
- **Task ID & Feature Reference**: `TASK-FE-401`, `TASK-FE-402` | Feature I (`FR-HIS-01~10`), Feature K, `ADR-01`
- **Description**: Scaffolding the standalone Web Management Portal application using React Router DOM (`/login`, `/register`, `/dashboard/*`). Build the comprehensive User Dashboard providing searchable reading history, domain filtering, reading time analytics, note/chat transcript export (Markdown, PDF, JSON), personal BYOK key vault, and GDPR data purge triggers.
- **Plan & Codebase**:
  - `src/web/router.tsx`: React Router configuration with protected route guards (`RequireAuth`, `RequireAdmin`).
  - `src/web/pages/dashboard/OverviewPage.tsx`: High-level metrics: articles read this week, total reading time, quiz accuracy rate, favorite domains.
  - `src/web/pages/dashboard/HistoryPage.tsx`: Searchable reading history table with date picker, domain chips, and dwell time.
  - `src/web/pages/dashboard/ExportModal.tsx`: 1-click exporter for notes and conversation transcripts to `.md`, `.json`, and printable styled PDF.
  - `src/web/pages/dashboard/PrivacyCenterPage.tsx`: GDPR management: "Clear All Browsing Logs" (`DELETE /reading/history`) and "Delete My Account" (`DELETE /users/me`) with modal confirmation and password challenge.
- **Scenarios & Edge Cases**:
  - *Exporting Huge History*: User with 1,000+ reading sessions exports to Markdown; data is streamed and assembled in a web worker using `Blob` URL to prevent browser tab freeze.
  - *Account Deletion*: User confirms account deletion; frontend purges all local storage tokens, terminates session, and redirects to goodbye page.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Provides users a spacious desktop environment for deep reflection, research synthesis, and data ownership rights.
  - 🛠️ **TechLead Lens**: TanStack Query table pagination (`useQuery` with `keepPreviousData: true`) ensures smooth navigation without flicker.
  - 🚀 **DevOps Lens**: Static SPA deploys effortlessly to Netlify, Vercel, or AWS S3 + CloudFront with standard single-page redirect rules.
  - 🏛️ **Software Architecture Lens**: Reuses 100% of API services and types from `src/shared/`, verifying `ADR-01` dual-delivery architecture.
- **Acceptance Criteria**:
  - Protected routes redirect unauthenticated users to `/login`.
  - History table filters accurately by search keywords, date ranges, and domains.
  - Export generates valid Markdown and JSON files downloaded to user machine.
  - Account deletion completely invalidates session and removes user data.
- **Verification / Screenshot**:
  - Command: `npm run dev:web` and browse to `http://localhost:5173/dashboard`.
  - Screenshot: User dashboard rendering reading analytics cards, history table, and export buttons.

---

### [FE-21] Admin Portal Telemetry, User Moderation & Quality Feedback Review
- **Task ID & Feature Reference**: `TASK-FE-403` | Feature L (`FR-OPS-01~05`), `ADR-01`
- **Description**: Build the Admin Management Portal (`/admin/*`) restricted strictly to users with the `admin` role. Features real-time system telemetry charts (AI request volume, provider distribution pie chart, error rates), user account moderation (search, view, toggle status: Active/Suspended/Banned), and user feedback inspection with heuristic PII redaction.
- **Plan & Codebase**:
  - `src/web/pages/admin/TelemetryDashboard.tsx`:
    - Telemetry metrics over selectable timeframe (`24h`, `7d`, `30d`).
    - Upstream AI provider breakdown (Gemini vs OpenAI vs Claude) rendered with Recharts.
    - Error rate tracking (429 Rate Limits, 401 Invalid Keys, 504 Upstream Timeouts).
    - **Strict Privacy Guarantee**: Displays numerical counters and aggregated stats only; zero plaintext reading text is accessible to admins.
  - `src/web/pages/admin/UserManagementPage.tsx`:
    - Paginated user table (ID, Email, Role, Created Date, Status).
    - Status toggle dropdown firing `PATCH /api/v1/admin/users/{id}/status`.
    - Guard preventing admins from modifying their own account status.
  - `src/web/pages/admin/FeedbackReviewPage.tsx`:
    - Review table of user ratings (helpful vs unhelpful) and report reasons (`hallucination`, `inaccurate`, `harmful`).
    - Client-side heuristic PII masking on user comment text.
- **Scenarios & Edge Cases**:
  - *Unauthorized Access Attempt*: Non-admin authenticated user attempts to browse directly to `/admin/telemetry`; protected route guard immediately redirects to `/dashboard` with an access denied toast.
  - *Admin Self-Ban Prevention*: Admin attempts to suspend their own account; UI blocks action and displays alert: *"Cannot modify your own administrator status"*.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Enables operational monitoring, quality control, and community moderation without compromising end-user privacy.
  - 🛠️ **TechLead Lens**: Role-based access control (RBAC) enforced both at React Router level and checked against backend claims.
  - 🚀 **DevOps Lens**: Telemetry queries use cached intervals to prevent putting unnecessary analytical query load on the primary PostgreSQL database.
  - 🏛️ **Software Architecture Lens**: Clean separation between user dashboard logic and administrative control planes.
- **Acceptance Criteria**:
  - Only users with `role: "admin"` can access `/admin/*` routes.
  - Telemetry charts render provider splits and error trends accurately.
  - Admin can update user status to Suspended/Banned, instantly reflecting in the UI.
  - Feedback table displays ratings with PII masked in comment fields.
- **Verification / Screenshot**:
  - Manual test: Log in as admin, test user status change and view telemetry charts.
  - Screenshot: Admin telemetry dashboard showing live request charts, provider breakdown pie chart, and user moderation table.

---

### [FE-22] Static Documentation, BYOK Handbook & Help Center
- **Task ID & Feature Reference**: `TASK-FE-404` | Feature C, System Documentation
- **Description**: Build the public Static Help Center & Documentation portal (`/docs/*`). Includes step-by-step walkthroughs for installing the unpacked extension, comprehensive guides for obtaining free BYOK API keys from Google AI Studio, OpenAI Platform, and Anthropic Console, troubleshooting handbooks, and accessibility keyboard shortcut references.
- **Plan & Codebase**:
  - `src/web/pages/docs/DocsLayout.tsx`: Documentation layout with sidebar navigation, search bar, and on-this-page table of contents.
  - `src/web/pages/docs/InstallationGuide.tsx`: Extension installation guide with annotated screenshots.
  - `src/web/pages/docs/BYOKGuide.tsx`: Step-by-step walkthroughs with deep links to Google AI Studio, OpenAI, and Anthropic console for generating API keys.
  - `src/web/pages/docs/AccessibilityFAQ.tsx`: Guide to OpenDyslexic typography, Bionic Reading science, and keyboard navigation shortcuts.
  - `src/web/pages/docs/TroubleshootingPage.tsx`: Explanations for common network errors, CSP blocks, and privacy features.
- **Scenarios & Edge Cases**:
  - *Searchable Documentation*: Readers searching "OpenAI key" or "Dyslexia" instantly filter relevant guides with highlighted search terms.
  - *Responsive Layout*: Documentation renders seamlessly across mobile, tablet, and widescreen desktop monitors.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Removes onboarding barriers for non-technical users, empowering anyone to easily set up their own API keys and extension.
  - 🛠️ **TechLead Lens**: Lightweight markdown/MDX components compiled statically for zero runtime overhead and fast search indexing.
  - 🚀 **DevOps Lens**: Zero backend dependencies; can be cached aggressively at CDN edge for 365 days.
  - 🏛️ **Software Architecture Lens**: Independent route structure that can be accessed without authentication.
- **Acceptance Criteria**:
  - Documentation pages render cleanly with sidebar navigation and active anchor highlighting.
  - Step-by-step BYOK guides contain accurate external links and security warnings.
  - Search bar filters documentation articles in real-time.
- **Verification / Screenshot**:
  - Command: Browse to `http://localhost:5173/docs/byok-setup`.
  - Screenshot: Documentation page displaying illustrated guide for Google Gemini API key creation.

---

## Phase 6: Quality Assurance, End-to-End Testing & Extension Packaging

### [FE-23] Comprehensive Unit & Integration Test Suite (Vitest & RTL)
- **Task ID & Feature Reference**: `TASK-FE-501`, `TASK-FE-502` | SRS §4.2, `NFR-REL-01~05`, `NFR-SEC-01~08`
- **Description**: Implement the complete automated test suite using **Vitest**, **React Testing Library (RTL)**, and **JSDOM**. Covers DOM extraction heuristics, sensitive data pre-flight scanner, SSE streaming chunk parser, Zustand state transitions, and component tests for ChatMessageItem, QuizCard, and Auth forms.
- **Plan & Codebase**:
  - `vitest.config.ts`: Vitest configuration with JSDOM environment, setup files, and coverage thresholds.
  - `src/tests/setup.ts`: Chrome API mock harness (`chrome.runtime`, `chrome.storage`, `chrome.tabs`, `chrome.sidePanel`).
  - Unit Tests:
    - `src/extension/content/__tests__/DOMExtractor.test.ts`: Tests article extraction on mocked HTML fixtures (Medium, Wikipedia, paywalled mock).
    - `src/shared/services/__tests__/sensitiveDataInspector.test.ts`: Verifies detection of API keys, credentials, and credit cards.
    - `src/shared/services/__tests__/sseStreamReader.test.ts`: Tests SSE reader against broken chunk boundaries and cancellation.
  - Component Tests:
    - `src/extension/sidepanel/__tests__/ChatMessageItem.test.ts`: Verifies markdown, KaTeX math, and citation pill rendering.
    - `src/extension/sidepanel/__tests__/QuizCard.test.ts`: Verifies option selection, submission, and score review rendering.
    - `src/shared/components/__tests__/AuthForms.test.ts`: Form validation and error states.
- **Scenarios & Edge Cases**:
  - *Broken Packet Simulation*: Test feeds SSE stream with tokens cut halfway through multi-byte UTF-8 sequences to verify no corrupted characters occur.
  - *Mock Chrome API Injections*: Tests run cleanly in headless CI environments without a live browser instance.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Verifies that every business requirement and user interaction contract behaves strictly according to specification.
  - 🛠️ **TechLead Lens**: High test coverage (**≥ 80%**) provides a safety net for rapid refactoring and feature additions.
  - 🚀 **DevOps Lens**: Vitest executes the entire 100+ test suite in < 10 seconds in CI/CD pipeline, failing fast on regressions.
  - 🏛️ **Software Architecture Lens**: Validates contract adherence across isolation boundaries (mocks verify message payloads match schemas).
- **Acceptance Criteria**:
  - All unit and component tests pass via `npm run test`.
  - Code coverage across services, utilities, and stores reaches **≥ 80%**.
  - Zero unhandled promise rejections or React act() warnings in test logs.
- **Verification / Screenshot**:
  - Command: `npm run test:coverage`
  - Screenshot: Terminal output showing 100% passing tests and coverage report table displaying ≥ 80% coverage.

---

### [FE-24] Production Bundle Optimization, Security Audit & Web Store Packaging
- **Task ID & Feature Reference**: `TASK-FE-503`, `TASK-FE-504` | Chrome Web Store Guidelines, `NFR-MNT-01~04`
- **Description**: Optimize production bundles, execute a security audit, and generate the final distribution package for Chrome Web Store submission. Enforce strict Content Script bundle budgets (< 150KB gzip), verify Content Security Policy headers, purge source maps, and package into a clean zip archive.
- **Plan & Codebase**:
  - `vite.config.ts`: Production build optimizations:
    - Code splitting for Side Panel, Options, Popup, and Web Portal.
    - Treeshaking unused Lucide icons and Radix primitives.
    - Terser minification stripping `console.log` and `debugger` statements.
  - Content Script Budget Enforcement: Injected content script bundle must remain < 150KB gzip to ensure instantaneous page load on host websites.
  - Security Audit:
    - Verify `manifest.json` requests only strictly necessary permissions.
    - Verify zero dangerous HTML sinks (`dangerouslySetInnerHTML` without DOMPurify sanitization).
    - Ensure zero plaintext secret keys or tokens exist in build outputs.
  - Packaging Script: `scripts/package-extension.js` creating `dist/readhead-extension-v1.0.0.zip` ready for Chrome Web Store developer dashboard upload.
- **Scenarios & Edge Cases**:
  - *Chrome Web Store Review Rejection*: Automated linters scan for remote script loading (`eval()`, dynamic CDN scripts). Extension must bundle 100% of code locally with strict `script-src 'self'`.
  - *Large Dependencies*: Heavy modules (like `pdfjs-dist`) are lazily loaded on-demand so they do not bloat the primary content script.
- **Senior 4-Lens Validation**:
  - 👔 **BA Lens**: Guarantees fast review and approval on the official Chrome Web Store, ensuring smooth market launch.
  - 🛠️ **TechLead Lens**: Bundle analyzer (`rollup-plugin-visualizer`) generates visual treemap to continuously monitor chunk sizes.
  - 🚀 **DevOps Lens**: Automated packaging script integrates into GitHub Actions release workflow to publish zipped artifacts on Git tags.
  - 🏛️ **Software Architecture Lens**: Validates final architectural compliance with Chrome Manifest V3 and ISO/IEC/IEEE 29148:2018 Verification Matrix.
- **Acceptance Criteria**:
  - Content script bundle size is < 150KB gzip.
  - Production build strips all debug logs and emits zero warnings.
  - Extension zip package installs cleanly in a clean Chrome browser profile.
  - Passes automated Chrome Web Store manifest and security audit checks.
- **Verification / Screenshot**:
  - Command: `npm run build && node scripts/package-extension.js`
  - Screenshot: Bundle analyzer treemap and generated `.zip` artifact details.
