# Knowledge — Constraints & Boundaries

## 1. Chrome Extension Manifest V3 Constraints
- **Ephemeral Background Service Worker**: Cannot maintain long-lived in-memory state or continuous WebSocket connections. Wakes on browser events, terminates when idle.
- **Content Security Policy (CSP)**: `eval()`, `new Function()`, and remote code execution are strictly blocked. All code must be bundled locally.
- **Storage Limits**: `chrome.storage.local` has a default quota of 10MB (or `unlimitedStorage` with permission). Avoid caching megabytes of raw uncompressed article HTML.

## 2. In-Page Coexistence Constraints
- **Host Page Style Pollution**: Third-party websites may use aggressive global resets (`* { box-sizing: border-box; margin: 0; }` or `!important` font styles). All extension injected UI must reside inside **Shadow DOM** boundaries.
- **Cross-Origin Iframes**: Extension content scripts cannot easily inspect content inside cross-origin `<iframe>` elements (e.g. embedded ads, external widgets). The DOM extractor must gracefully ignore unreadable iframes.

## 3. Grounding & Zero-Hallucination Policy (SRS §3.3.3 NFR-AI)
- When answering questions about an article, the AI output must prioritize on-page text evidence.
- The UI must visually differentiate between confirmed on-page citations vs general knowledge synthesized by the LLM.
