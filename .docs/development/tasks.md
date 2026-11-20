# Phased Development Tasks — ReadHead-FE

This document tracks all frontend engineering tasks required to fulfill the 12 SRS feature specifications (Features A through L) and the extended Web Management Portal.

---

## Phase 1 — Project Scaffolding & Shared Infrastructure
- [x] **TASK-FE-101**: Establish project context, rules (`.agents/rules/`), and documentation hierarchy (`.docs/`).
- [ ] **TASK-FE-102**: Initialize `package.json`, Vite configuration, TypeScript `tsconfig.json`, and `@crxjs/vite-plugin` manifest configuration.
- [ ] **TASK-FE-103**: Configure Tailwind CSS, PostCSS, and Shadcn/ui directory scaffolding.
- [ ] **TASK-FE-104**: Define TypeScript data types matching ReadHead-BE Pydantic models (`src/shared/types/`).
- [ ] **TASK-FE-105**: Implement typed Axios API client with automatic JWT token refresh interceptor (`src/shared/services/apiClient.ts`):
  - [ ] Automatic silent token refresh on 401 with 30s concurrent refresh grace period support.
  - [ ] Immediate purge of local session upon receiving `401 USER_SUSPENDED` or `401 REFRESH_TOKEN_REVOKED`.
- [ ] **TASK-FE-106**: Implement native SSE streaming reader with `fetch` and `ReadableStream` (`src/shared/services/sseStreamReader.ts`):
  - [ ] Parse typed events (`start`, `token`, `citation`, `done`, `error`).
  - [ ] AbortController support for client-side cancellation with partial response preservation.
  - [ ] Enforce client-side guard for max 2 active concurrent streams (`429 STREAM_CONCURRENCY_EXCEEDED`).
- [ ] **TASK-FE-107**: Implement Zustand global stores: `useAuthStore`, `useTabContextStore`, and `useSettingsStore`.
- [ ] **TASK-FE-108**: Implement client storage abstraction layer (`chrome.storage.local` + `localStorage`).

---

## Phase 2 — Chrome Extension Surfaces (Manifest V3)
- [ ] **TASK-FE-201**: Implement Background Service Worker (`background.ts`) managing tab events, alarms, and cross-context routing.
- [ ] **TASK-FE-202**: Implement Content Script text selection detector and Shadow DOM floating action menu (`SelectionWatcher`, `FloatingActionMenu`).
- [ ] **TASK-FE-203**: Implement in-page clean article extraction using `@mozilla/readability` (`DOMExtractor.ts`):
  - [ ] SPA History API interceptor (`pushState`, `popstate`, `MutationObserver` on URL/title changes).
  - [ ] Tiered fallback for paywalls and canvas: Readability -> high-density text node scanning -> user text selection prompt.
  - [ ] Cross-origin iframe boundary detection (bypasses inaccessible iframes, prompts user selection).
  - [ ] Local client-side PDF text extraction using bundled Mozilla `pdf.js` (max 100 KB text extract, zero raw binary upload).
- [ ] **TASK-FE-204**: Implement in-page accessibility modifiers (`AccessibilityRenderer.ts`):
  - [ ] Bundle OpenDyslexic `.woff2` assets via `chrome-extension://` to bypass host site Content Security Policies (CSP).
  - [ ] Bionic reading engine restricted to pure `Node.TEXT_NODE` within content tags; skips interactive elements (`<a>`, `<button>`, `<code>`, `<input>`).
  - [ ] `WeakMap<Node, string>` snapshot for 1-click instant, non-destructive DOM undo.
- [ ] **TASK-FE-205**: Implement local-first dwell time & reading pace tracker (`DwellTracker.ts`):
  - [ ] Idle detection guard: pauses timer after 45s of user inactivity (no scroll/keyboard/mouse) or when tab is hidden.
  - [ ] Paragraph threshold filter: measures only `<p>` nodes with >= 25 words or >= 150 characters.
  - [ ] Dismissal fatigue policy: 5-minute cooldown on domain and 3-paragraph suppression upon user closing a hint chip.
  - [ ] Offline silence: suppresses proactive triggers when `navigator.onLine = false`.
- [ ] **TASK-FE-206**: Build Chrome Side Panel UI (`sidepanel.tsx`):
  - [ ] Tab header & Model selector (`Gemini`, `OpenAI`, `Claude`).
  - [ ] Chat conversation feed with markdown, syntax highlighting, and KaTeX math.
  - [ ] Real-time token streaming with animated typing cursor.
  - [ ] Grounded citation pills with host page auto-scroll trigger.
  - [ ] Thumbs up/down feedback ratings.
- [ ] **TASK-FE-207**: Implement Interactive Comprehension Quiz view (`QuizCard.tsx`) with instant answer grading and rationale display.
- [ ] **TASK-FE-208**: Build Action Popup (`popup.tsx`) for fast status inspection and one-click Side Panel toggle.
- [ ] **TASK-FE-209**: Build Options Page (`options.tsx`) for BYOK key vault management and domain blacklist configuration.

---

## Phase 3 — Privacy, Guardrails & Security
- [ ] **TASK-FE-301**: Build client-side pre-flight PII / credential detector (`sensitiveDataInspector.ts`) with default-block modal.
- [ ] **TASK-FE-302**: Implement draft preservation & safe reconnection (`draftRecoveryService.ts`):
  - [ ] Debounced (300ms) continuous autosave to `chrome.storage.local` keyed by `draft_<tabId>`.
  - [ ] Automatic draft restoration into Side Panel input upon tab switch, browser restart, or token expiry.
  - [ ] Strict Anti-Auto-Retry compliance (`FR-ERR-04`): Online recovery displays connectivity toast, never auto-resubmits pending requests.
- [ ] **TASK-FE-303**: Implement domain blacklist check in background worker to completely silence extension on sensitive origins.
- [ ] **TASK-FE-304**: Implement masked key presentation and secret isolation (ensure plain keys are never logged in console or stored unencrypted on client).
- [ ] **TASK-FE-305**: Implement multi-account logout purge protocol (`FR-ACC-04`):
  - [ ] Wipe `chrome.storage.local` tokens, drafts, and cached article extracts.
  - [ ] Broadcast `ACTION_PURGE_SESSION` across all open Side Panels, Popups, and Content Scripts.
  - [ ] Reset all Zustand stores and abort pending fetch calls via `AbortController`.
- [ ] **TASK-FE-306**: Implement multi-device sync conflict resolution modal (`ConflictResolverModal.tsx`):
  - [ ] Intercept `409 SYNC_CONFLICT` containing `server_record` and `client_version`.
  - [ ] Present visual diff dialog allowing user to choose "Keep Local Version" or "Accept Cloud Version".
- [ ] **TASK-FE-307**: Implement offline IndexedDB sync outbox queue (`sync_outbox`):
  - [ ] Cap local queue at 500 actions max (FIFO eviction with warning at 90% capacity).
  - [ ] Batch sync in chunks of 100 items upon network reconnection matching BE `limit: 100`.

---

## Phase 4 — Web Management Portal (User & Admin)
- [ ] **TASK-FE-401**: Setup React Router DOM structure (`/login`, `/register`, `/dashboard/*`, `/admin/*`, `/docs/*`).
- [ ] **TASK-FE-402**: Build User Dashboard:
  - [ ] Reading history explorer with search and domain filtering.
  - [ ] Note and conversation export to Markdown and JSON.
  - [ ] Personal BYOK key vault manager.
  - [ ] Account profile & GDPR data purge ("Clear All History", "Delete Account").
- [ ] **TASK-FE-403**: Build Admin Management Portal:
  - [ ] System telemetry overview (live request volume, AI provider distribution pie chart, error rates).
  - [ ] User management table with pagination, search, and status toggle (Active/Suspended/Banned).
  - [ ] User satisfaction & feedback review table.
- [ ] **TASK-FE-404**: Build Static Documentation & Help Center (`/docs/*`):
  - [ ] Extension installation walkthrough.
  - [ ] BYOK guide for obtaining Google, OpenAI, and Anthropic keys.
  - [ ] Accessibility features guide & FAQ.

---

## Phase 5 — Testing, Optimization & Quality Assurance
- [ ] **TASK-FE-501**: Write Vitest unit tests for DOM extractor, sensitive text detector, and SSE reader.
- [ ] **TASK-FE-502**: Write React Testing Library tests for ChatConversation, QuizCard, and Auth forms.
- [ ] **TASK-FE-503**: Measure and optimize Content Script bundle size (< 150KB gzip).
- [ ] **TASK-FE-504**: Verify against ISO/IEC/IEEE 29148:2018 SRS Verification Matrix (`HCI (5).md` §4).
