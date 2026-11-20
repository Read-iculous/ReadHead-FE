# Rule 08 — Chrome Extension Manifest V3 Conventions

## 1. Manifest V3 Constraints
- **Declarative Permissions**: Only request necessary permissions in `manifest.json`:
  - `activeTab` — allows reading DOM of currently active tab when user invokes action.
  - `sidePanel` — opens Side Panel UI alongside web content.
  - `storage` — persistent local client storage.
  - `scripting` — for optional programmatic injection when necessary.
- **Service Worker Lifecycle**:
  - Background script runs as an ephemeral ES Module service worker.
  - Never hold persistent WebSocket or long polling connections inside the background service worker without expecting termination.
  - Use `chrome.alarms` for periodic sync rather than `setInterval`.

## 2. Cross-Context Message Passing
- **Content Script <-> Background**: Use `chrome.runtime.sendMessage` and `chrome.runtime.onMessage`.
- **Side Panel <-> Content Script**: Route through background worker or query the active tab directly via `chrome.tabs.sendMessage(activeTabId, ...)`.
- All messages must have a typed discriminator `type` field and typed payload:
  ```ts
  export type ExtensionMessage =
    | { type: 'EXTRACT_PAGE_CONTENT'; tabId: number }
    | { type: 'PAGE_CONTENT_EXTRACTED'; payload: ExtractedContent }
    | { type: 'SELECTION_ACTION'; action: 'explain' | 'summarize' | 'translate'; text: string }
    | { type: 'PROACTIVE_HINT_TRIGGERED'; hint: ProactiveHint };
  ```

## 3. DOM Injection Isolation
- Injected UI components (Tooltip, Floating Actions) inside the host web page must be wrapped in a **Shadow DOM** (`attachShadow({ mode: 'closed' })`) to prevent host page CSS styles from breaking our UI, and vice-versa.
