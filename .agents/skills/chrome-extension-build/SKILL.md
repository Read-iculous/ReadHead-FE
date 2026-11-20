---
name: chrome-extension-build
description: Workflow for configuring, testing, and bundling Chrome Extension Manifest V3 targets in ReadHead-FE.
---

# Chrome Extension Build & Debug Skill

## When to Use
Use when modifying `manifest.json`, background service worker lifecycles, content script injection, or packaging extension builds for testing.

## Step-by-Step Procedure

1. **Manifest V3 Verification**:
   - Check `public/manifest.json`.
   - Ensure permissions are strictly minimal (`activeTab`, `sidePanel`, `storage`).
   - Ensure `background.service_worker` is specified as an ES module (`"type": "module"`).

2. **Run Dev Build**:
   ```bash
   pnpm run dev
   ```

3. **Load Unpacked in Chrome**:
   - Open `chrome://extensions/`.
   - Enable Developer Mode.
   - Click "Load Unpacked" and select `ReadHead-FE/dist`.

4. **Verify Lifecycles**:
   - Inspect the Background Service Worker via Chrome DevTools.
   - Test tab activation events to ensure per-tab state updates without memory leaks.
   - Test Content Script Shadow DOM injection on a live article.
