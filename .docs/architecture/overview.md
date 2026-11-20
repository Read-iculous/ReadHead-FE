# Architecture Overview — ReadHead-FE

## 1. Architectural Style & Philosophy
ReadHead-FE is engineered around a **Modular, Dual-Target Client Architecture**:
- **Zero-Friction Coexistence**: The Chrome Extension must never pollute the global scope or CSS styles of host websites. Content scripts encapsulate all injected UI inside **Shadow DOM** boundaries.
- **Strict Per-Tab Context Isolation (FR-CNT-04)**: Reading context, selected snippets, and chat sessions are keyed to the specific browser `tabId`. Switching tabs immediately swaps the active context in the Side Panel, preventing cross-tab pollution.
- **Local-First Resilience**: Draft messages, active tab readings, and token states are cached in `chrome.storage.local`. In the event of network dropouts or backend restarts, client state remains intact.
- **Separation of Presentation, State, and Transport**:
  - Presentation: Dumb UI components driven by Shadcn/ui & Tailwind CSS.
  - Client State: Zustand stores managing synchronous UI configurations.
  - Server State: TanStack Query managing async data caching and optimistic updates.
  - Transport: Dedicated typed API service layer with SSE streaming listener.

---

## 2. Chrome Extension Architecture (Manifest V3)

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           CHROME EXTENSION RUNTIME                          │
├────────────────────────────────┬────────────────────────────────────────────┤
│ 1. Content Script Context      │ 2. Background Context                      │
│ - Injected into active webpage │ - Manifest V3 Service Worker               │
│ - Text Selection Listener      │ - Lifecycle & Alarm Management             │
│ - Dwell Time & Scroll Tracker  │ - Cross-Tab Routing Hub                    │
│ - Shadow DOM Floating Tooltip  │ - Chrome Storage Persistence               │
│ - Bionic & Font Re-renderer    │                                            │
├────────────────────────────────┴────────────────────────────────────────────┤
│ 3. Extension UI Context (Dedicated Windows / Views)                         │
│ ┌─────────────────────────┐  ┌───────────────────────┐  ┌────────────────┐ │
│ │ Side Panel (Main View)  │  │ Action Popup          │  │ Options Page   │ │
│ │ - AI Chat Conversation  │  │ - Quick Status Check  │  │ - BYOK Vault   │ │
│ │ - SSE Stream Viewer     │  │ - Reading Mode Toggle │  │ - Blacklist    │ │
│ │ - Quiz Generation View  │  │ - Open Sidepanel Btn  │  │ - Preferences  │ │
│ └─────────────────────────┘  └───────────────────────┘  └────────────────┘ │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 3. Web Management Portal Architecture
The web portal operates as a single-page web app built with Vite + React:
- **Public Routes**: `/login`, `/register`, `/docs/*` (Static Help Center & Guides).
- **User Protected Routes (`/dashboard/*`)**:
  - `/overview`: Reading statistics, comprehension scores, recent active documents.
  - `/history`: Full searchable conversation history with sync controls.
  - `/keys`: Personal BYOK configuration vault.
  - `/settings`: Account details, reading accessibility defaults, data deletion.
- **Admin Protected Routes (`/admin/*`)**:
  - `/telemetry`: Platform metrics, AI provider breakdown, error rates.
  - `/users`: User management table (Active / Suspended / Banned).
  - `/feedback`: User satisfaction log and problem reports.
