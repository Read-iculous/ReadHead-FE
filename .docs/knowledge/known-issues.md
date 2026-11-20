# Knowledge — Known Issues & Edge Cases

## 1. Single Page Applications (SPAs) & Client-Side Navigation
- **Issue**: Modern web apps (e.g. Twitter/X, Medium, Substack) navigate between articles without triggering full browser page reload events (`chrome.tabs.onUpdated`).
- **Mitigation**: The Content Script must listen to `popstate` and mutation observer events on `document.title` and `location.href` to re-extract article content when URLs change client-side.

## 2. Heavy Dynamic Paywalls & Protected Domains
- **Issue**: Some news sites (e.g. Bloomberg, WSJ, NYTimes) inject obfuscated DOM structures or full-screen paywall modals that obscure the underlying text.
- **Mitigation**: `DOMExtractor` falls back to capturing user-selected text (`window.getSelection()`) if whole-page article extraction yields empty or gated results.

## 3. Chrome Side Panel Lifecycle
- **Issue**: In Chrome, opening the Side Panel for one tab may cause it to remain visible when switching to another tab.
- **Mitigation**: The Side Panel listens to `chrome.tabs.onActivated` and swaps the active conversation context immediately to avoid tab context confusion.
