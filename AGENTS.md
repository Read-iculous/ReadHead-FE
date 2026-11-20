# ReadHead-FE — Agent Context

## What This Project Is
**ReadHead-FE** is the frontend client ecosystem for the **Intelligent Reading Assistant**, an AI-powered reading assistance system specified under ISO/IEC/IEEE 29148:2018 standards.
It consists of two primary frontend deliverables:
1. **Chrome Browser Extension (Manifest V3)**: The primary client embedding directly into the user's reading workflow. Provides side-panel AI chat, text selection quick-action tooltips, in-page accessibility re-rendering (OpenDyslexic, bionic reading, line spacing), BYOK vault management, comprehension quizzes, and non-intrusive proactive reading assistance.
2. **Web Management Portal & Static Docs (Web Dashboard)**:
   - **User Portal**: Account settings, reading statistics, synced history review, BYOK configuration, and data export/deletion.
   - **Admin Portal**: System health telemetry, anonymized usage metrics (AI provider split, error rate monitoring, helpfulness ratings), and user account lifecycle management (active/banned).
   - **Static Help Site**: Setup guides, troubleshooting, FAQs, and privacy documentation.

## Tech Stack
| Layer | Technology | Purpose |
|---|---|---|
| Language | TypeScript 5.0+ | Strict type safety, shared types with BE Pydantic schemas |
| Core Framework | React 18+ | Declarative component UI |
| Build Tool | Vite 5+ (with `@crxjs/vite-plugin` for extension) | Lightning-fast HMR and bundle optimization |
| Styling | Tailwind CSS + Shadcn/ui (Radix UI primitives) | Modern, responsive, accessible design system |
| Icons | Lucide React | Clean, lightweight icon suite |
| State Management | Zustand | Global client state (auth session, user preferences, active tabs) |
| Server State & Caching | TanStack Query (React Query v5) | Cache, background refetch, optimistic updates |
| AI Protocol | SSE Client Listener (`fetch` + `ReadableStream`) | Real-time text chunk streaming with token stats |
| Client Storage | `chrome.storage.local` + `chrome.storage.session` / `localStorage` | Token caching, drafts, offline fallback |
| Backend Integration | ReadHead-BE (FastAPI async, PostgreSQL, Redis) | REST endpoints + SSE stream via JWT Bearer Auth |

## Mandatory Rules — Read First
1. **Always read `.docs/` before writing any code** — no guessing allowed.
2. **Strict Manifest V3 Compliance** — never use `eval()`, never inject unsafe inline scripts, adhere to Chrome Extension service worker lifecycles.
3. **Never expose or leak API Keys** — user BYOK keys are handled in memory and transmitted directly to BE encrypted vault; never log keys in client console or store in unencrypted plain cookies.
4. **Context Isolation & Tab Boundary** — extension content scripts and sidepanels must maintain strict per-tab isolation (`activeTab` context). Never cross-contaminate chat context between different browser tabs.
5. **Client-side Non-intrusive Monitoring** — proactive reading detection (dwell time, reading pace) runs locally in content script via lightweight heuristics; never stream continuous user tracking or heavy telemetry to servers.
6. **Ask before guessing** — if UI behavior, API contracts, or business edge cases are unclear, stop and ask the user.

## Codebase Map (Planned Structure)
```
ReadHead-FE/
├── src/
│   ├── extension/              ← Chrome Extension (Manifest V3)
│   │   ├── background/         ← Service Worker (lifecycle, tab messaging, alarms)
│   │   ├── content/            ← Injected Content Scripts (DOM extraction, tooltip, accessibility)
│   │   ├── sidepanel/          ← Primary Side Panel UI (AI Chat, Quiz, Summary, SSE)
│   │   ├── popup/              ← Extension Action Popup (Quick status, toggle, auth trigger)
│   │   └── options/            ← Dedicated Extension Settings & BYOK Vault page
│   ├── web/                    ← Web Management Portal (User & Admin Dashboard)
│   │   ├── pages/              ← Dashboard, Analytics, User Management, Docs
│   │   └── layouts/            ← AdminLayout, UserLayout, DocsLayout
│   ├── shared/                 ← Shared utilities, hooks, UI components & API client
│   │   ├── components/         ← Shadcn/ui & custom UI components
│   │   ├── hooks/              ← useAuth, useSSE, useReadingTracker, useStorage
│   │   ├── services/           ← Axios/fetch API client, SSE stream reader
│   │   ├── stores/             ← Zustand stores (authStore, settingsStore, tabStore)
│   │   ├── types/              ← TypeScript interfaces matching BE Pydantic schemas
│   │   └── utils/              ← Sensitive text detector, DOM parser, crypto
├── public/                     ← Extension manifest.json, icons, static assets
├── .agents/                    ← AI Agent rules and specialized workflows
│   ├── rules/                  ← Core development rules
│   └── skills/                 ← Step-by-step implementation skills
└── .docs/                      ← Comprehensive architectural & technical documentation
```
