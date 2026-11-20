# Component Architecture — ReadHead-FE

## 1. Extension Components Breakdown

### 1.1 In-Page Content Script (`src/extension/content/`)
- **`SelectionWatcher`**: Listens for user text highlights; calculates coordinates for floating tooltip.
- **`FloatingActionMenu` (Shadow DOM)**: Renders quick-action buttons: *Explain*, *Summarize*, *Translate*, *Quiz*.
- **`DwellTracker`**: Measures local paragraph dwell time and scroll activity. Features idle detection (pauses when inactive > 45s or tab hidden), minimum word threshold (>= 25 words), 5-minute dismissal cooldown, and offline suppression.
- **`AccessibilityRenderer`**: Modifies typography on demand. Injects local OpenDyslexic font assets via `chrome-extension://` to bypass host CSP, applies Bionic Reading exclusively to read-only text nodes while preserving host DOM and interactive elements, and supports 1-click full undo.
- **`DOMExtractor`**: Extracts clean article text and metadata using `@mozilla/readability`. Implements multi-tier fallback for gated paywalls/canvas, hooks into SPA navigation events (`popstate`, `MutationObserver`), and handles cross-origin iframe boundaries.

### 1.2 Side Panel (`src/extension/sidepanel/`)
- **`SidePanelApp`**: Root container responding to active tab changes.
- **`ChatConversation`**: Message list rendering user prompts, AI responses, timestamps, and model badges.
- **`StreamMessageBlock`**: Renders live incoming SSE tokens, parses markdown formatting, highlights code blocks, and renders KaTeX formulas.
- **`GroundedCitationPill`**: Displays clickable citation badges linking AI answers to specific paragraphs on the active webpage.
- **`QuizCard`**: Renders multiple-choice comprehension questions with explanation reveal upon selection.
- **`SensitiveDataAlertModal`**: Warning dialog intercepting prompts containing PII/credentials before dispatching.

### 1.3 Popup & Options (`src/extension/popup/` & `src/extension/options/`)
- **`QuickStatusCard`**: Displays current active user, selected AI provider, and extension enabled/disabled toggle.
- **`ApiKeyVaultManager`**: Interface for adding/updating BYOK keys with real-time validation badges.
- **`DomainBlacklistEditor`**: Allows users to specify websites where the extension must stay completely inactive.

---

## 2. Shared Subsystems (`src/shared/`)

### 2.1 Services & API Client (`src/shared/services/`)
- **`apiClient`**: Axios/Fetch client configured with interceptors for JWT access token injection and automatic refresh token rotation.
- **`sseStreamService`**: Wraps `fetch` + `ReadableStream` to consume `POST /api/v1/ai/stream`. Emits typed events (`token`, `citation`, `error`, `done`).
- **`storageService`**: Abstraction over `chrome.storage.local` (in extension) and `localStorage` (in web app).

### 2.2 Global Zustand Stores (`src/shared/stores/`)
- **`useAuthStore`**: User identity, JWT tokens, login/logout, session expiry listener.
- **`useSettingsStore`**: Active AI provider, proactive assistance sensitivity, dark/light theme, accessibility flags.
- **`useTabContextStore`**: Active `tabId`, extracted article context, selected snippet, active draft text.

### 2.3 UI Design System (`src/shared/components/ui/`)
- Accessible primitives powered by **Shadcn/ui** (Dialog, DropdownMenu, Button, Input, Switch, Tooltip, Progress, Toast, Tabs).
