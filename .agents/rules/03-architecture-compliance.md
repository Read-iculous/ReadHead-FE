# Rule 03 — Architecture Compliance

## Architectural Boundaries

### 1. Dual-Surface Separation
The repository serves two distinct targets:
- **Target 1 — Chrome Extension**:
  - Entry points: `background.ts` (service worker), `content.ts` (DOM injection), `sidepanel.tsx` (primary UI), `popup.tsx` (quick actions), `options.tsx` (full settings).
  - Must not rely on server-side rendering or node-specific modules.
  - Must handle `chrome.runtime` and `chrome.storage` asynchronously.
- **Target 2 — Web Management Portal**:
  - Standard Single Page App (SPA) served via Vite.
  - Dedicated routes for `/dashboard`, `/admin`, `/docs`, `/login`.

### 2. State & Data Layering
- **Server Cache Layer (TanStack Query)**:
  - Used for fetching, caching, and synchronizing data from ReadHead-BE (conversations, user profile, API key list, quiz stats).
  - Stale time and garbage collection tuned for reading workflows.
- **Client Session Layer (Zustand)**:
  - Used for fast, synchronous UI state: active reading tab ID, sidepanel open/closed, current active selection snippet, UI theme.
- **Persistent Storage Layer (`chrome.storage.local` / `localStorage`)**:
  - JWT tokens (with auto-refresh cycle), offline unsent chat drafts, and local reading preferences.

### 3. Separation of Concerns
- **Components** must only handle presentation and user interactions.
- **Hooks** encapsulate business logic and storage sync.
- **Services** encapsulate raw network calls (`fetch`, `EventSource`, Chrome Message Passing).
