# Knowledge — Known Issues, Edge Cases & Operational Scenarios

This document catalogues all operational edge cases, real-world failure scenarios, and their explicit technical resolutions for ReadHead-FE.

---

## 1. Single Page Applications (SPAs) & Client-Side Navigation
- **Scenario**: Modern web applications (e.g. Substack, Medium, Twitter/X, GitHub Wiki) navigate between documents via the HTML5 History API (`pushState`, `replaceState`) without firing standard browser reload events (`chrome.tabs.onUpdated`).
- **Risk**: The extension retains stale article context and sends outdated page text or mismatched `page_url` to the backend.
- **Resolution**:
  - The Content Script intercepts `popstate`, `hashchange`, and uses a `MutationObserver` on `document.title` and `location.href`.
  - When a URL path change is detected, the Content Script invalidates the local article cache, re-invokes `DOMExtractor`, and dispatches an internal `TAB_URL_CHANGED` event to the Background Worker to reset the active `conversation_id`.

---

## 2. Dynamic Paywalls, Anti-Scraping & Obfuscated DOMs
- **Scenario**: News platforms (e.g. Bloomberg, WSJ, NYTimes) render obfuscated canvas text, inject full-screen paywall modals, or remove body text dynamically upon detecting dev tools or extensions.
- **Risk**: Automated whole-page extraction (`@mozilla/readability`) yields empty or truncated text, causing `422 UNREPRESENTATIVE_SOURCE`.
- **Resolution**:
  - `DOMExtractor` implements a tiered fallback:
    1. Primary: `@mozilla/readability` parses `<article>` or `<main>`.
    2. Fallback 1: Extract visible text nodes from high-density paragraph containers (`div`, `section`).
    3. Fallback 2: If extracted text is < 150 characters, prompt the user with an in-panel banner: *"This page is gated or protected. Please highlight specific text directly on the page to use ReadHead."* Active text selection (`window.getSelection()`) always supersedes whole-page extraction.

---

## 3. Chrome Side Panel Cross-Tab Persistence
- **Scenario**: In Google Chrome, opening the Side Panel (`chrome.sidePanel`) attaches it to the browser window. When the user switches tabs, the panel remains open, displaying the chat history of the previous tab.
- **Risk**: User asks a question about Tab B, but prompt context from Tab A is transmitted, causing context pollution or a `400 TAB_CONTEXT_MISMATCH` rejection.
- **Resolution**:
  - `SidePanelApp` subscribes to `chrome.tabs.onActivated`.
  - The instant the active tab changes, the UI transitions to a brief skeleton loader, pulls the active tab's `tabId` and `url` from `useTabContextStore`, and re-keys the chat timeline.
  - If the new tab has no existing conversation, a clean prompt interface is presented with quick-action chips (*"Summarize this article"*, *"Key Takeaways"*).

---

## 4. Dwell Time Idle Triggers, Inactivity & Dismissal Fatigue (Feature G)
- **Scenario 1 (User Idle)**: The user leaves their desk while reading a paragraph. The dwell timer continues incrementing, falsely concluding that the user is struggling with dense text and spamming proactive hints.
- **Scenario 2 (Dismissal Fatigue)**: The user dismisses a proactive hint because they prefer reading uninterrupted; the system triggers another suggestion on the next paragraph 20 seconds later.
- **Resolution**:
  - **Inactivity Guard**: `DwellTracker` listens for mouse movement, key presses, and scroll events. If zero user input is detected for > 45 seconds, or if `document.hidden = true` (tab switched or minimized), the timer is immediately paused.
  - **Minimum Content Length**: Dwell time is only measured for paragraphs (`<p>`) containing at least 25 words or 150 characters (ignoring headers, footers, nav links, and captions).
  - **Cooldown / Snooze Policy**: If a user dismisses a proactive hint chip, a 5-minute cooldown is activated across the active domain, and suggestions are suppressed for the next 3 consecutive paragraphs.
  - **Offline Silence**: Proactive hints are automatically disabled when `navigator.onLine = false`.

---

## 5. Strict Website Content Security Policy (CSP) vs OpenDyslexic Typography
- **Scenario**: Highly secure websites (e.g. banking portals, GitHub, government sites) enforce strict CSP headers (`font-src 'self'`, `style-src 'self'`) blocking third-party fonts or remote CDN stylesheets.
- **Risk**: Injected OpenDyslexic `@font-face` rules fail to load, resulting in broken typography or silent fallback to Times New Roman.
- **Resolution**:
  - The OpenDyslexic `.woff2` font files are bundled directly inside the extension package.
  - Declared in `manifest.json` under `web_accessible_resources`.
  - Font rules are injected referencing the secure local URL scheme: `chrome-extension://<EXTENSION_ID>/assets/fonts/OpenDyslexic-Regular.woff2`. This bypasses remote host CSP restrictions while maintaining style isolation.

---

## 6. Bionic Reading vs Virtual DOM Component Mutation Conflicts
- **Scenario**: Bionic Reading alters text by splitting words and wrapping the first half in `<b>` tags. If executed directly on the host DOM of a dynamic React/Vue application, it mutates nodes managed by the host framework's Virtual DOM, throwing catastrophic exceptions (e.g. `NotFoundError: Failed to execute 'removeChild' on 'Node'`).
- **Risk**: Crashing host single-page applications or corrupting hyperlinked text.
- **Resolution**:
  - `AccessibilityRenderer` strictly targets text nodes (`Node.TEXT_NODE`) within read-only content containers (`<article>`, `<p>`).
  - It explicitly skips interactive and executable elements: `<a>`, `<button>`, `<input>`, `<textarea>`, `<code>`, `<pre>`, `<script>`, `<style>`.
  - The original text content of every modified node is preserved in a `WeakMap<Node, string>`. When the user toggles Bionic Reading off, the original text nodes are restored instantly without triggering DOM rebuilds.

---

## 7. Multi-Account Browser Isolation & Cross-Account Leaks (FR-ACC-04)
- **Scenario**: User A logs out of ReadHead on a shared laptop. User B immediately logs into their account on the same Chrome profile.
- **Risk**: User B sees User A's un-sent question drafts, cached article extracts, or offline sync queue.
- **Resolution**:
  - Upon invoking `logout()`:
    1. Background Service Worker purges `chrome.storage.local` keys: `pending_prompt_draft`, `cached_article_context`, `access_token`, `refresh_token`.
    2. Background Worker broadcasts an internal runtime message `ACTION_PURGE_SESSION` to all open contexts (Side Panel, Options, Content Scripts).
    3. The Side Panel immediately resets Zustand stores to initial state and redirects to the `/login` view.
    4. In-flight requests are aborted via `AbortController`.

---

## 8. Network Dropout, Offline Draft Preservation & Safe Reconnect (FR-ERR-04, FR-ACC-06)
- **Scenario**: While composing a complex question or receiving an AI explanation, the internet connection drops or the laptop goes to sleep.
- **Risk**: User loses paragraphs of carefully written questions, or upon reconnection the client fires repeated duplicate requests causing double billing or LLM quota exhaustion.
- **Resolution**:
  - **Continuous Draft Autosave**: User input in the Side Panel prompt box is debounced (300ms) and saved to `chrome.storage.local` keyed by `draft_<tabId>`.
  - **On Expiry / Disconnect**: If an auth 401 occurs or connection is severed, the draft remains visible and editable.
  - **Strict Anti-Auto-Retry Policy (`FR-ERR-04`)**: When internet connectivity resumes (`window.addEventListener('online')`), the system shows a green connectivity toast: *"Internet connection restored"*. The system **NEVER** automatically re-dispatches the pending prompt. The user must explicitly review their draft and click the "Send" button.

---

## 9. Cross-Origin Iframes & Sandboxed Embeds
- **Scenario**: An article is rendered inside a cross-origin `<iframe>` (e.g. embedded Google Docs, Substack embeds, embedded PDFs in sandboxed viewers).
- **Risk**: Chrome Extension Content Scripts cannot access `iframe.contentDocument` due to the browser Same-Origin Policy.
- **Resolution**:
  - The Content Script checks `window !== window.top`. For nested iframes with cross-origin boundaries, automated full-page extraction is bypassed.
  - The UI displays a guidance tooltip: *"This reading section is in an isolated frame. Please highlight the text you want explained, or open the link directly in a separate tab."*
