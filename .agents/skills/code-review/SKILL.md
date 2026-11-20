---
name: code-review
description: Comprehensive checklist for reviewing frontend code in ReadHead-FE for Manifest V3 safety, accessibility, and architectural compliance.
---

# Code Review Skill

## When to Use
Use before finalizing any frontend feature or submitting code for integration.

## Review Checklist

### 1. Security & Privacy
- [ ] No plaintext API keys logged to `console.log` or stored in unencrypted cookies.
- [ ] No `eval()` or `new Function()` in extension code (strict Manifest V3 CSP).
- [ ] Client-side PII detector runs before any prompt is sent to AI stream.

### 2. Chrome Extension Compliance
- [ ] Content Script UI elements are fully encapsulated inside closed Shadow DOM.
- [ ] Clean event listener teardown in `useEffect` cleanup functions.
- [ ] Background service worker handles lifecycle termination gracefully (no stale in-memory state).

### 3. TypeScript & Code Quality
- [ ] Zero `any` types; all props and API payloads are strictly typed.
- [ ] Clean component structure (< 200 lines per component recommended).
- [ ] Accessible ARIA attributes and keyboard shortcuts included.

### 4. Documentation & Tasks
- [ ] `.docs/` files updated if contracts, variables, or architecture changed.
- [ ] Task marked as completed in `.docs/development/tasks.md`.
