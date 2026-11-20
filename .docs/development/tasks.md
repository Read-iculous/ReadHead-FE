# Phased Development Tasks — ReadHead-FE

This document tracks all frontend engineering tasks required to fulfill the 12 SRS feature specifications (Features A through L) and the extended Web Management Portal.

---

## Phase 1 — Project Scaffolding & Shared Infrastructure
- [x] **TASK-FE-101**: Establish project context, rules (`.agents/rules/`), and documentation hierarchy (`.docs/`).
- [ ] **TASK-FE-102**: Initialize `package.json`, Vite configuration, TypeScript `tsconfig.json`, and `@crxjs/vite-plugin` manifest configuration.
- [ ] **TASK-FE-103**: Configure Tailwind CSS, PostCSS, and Shadcn/ui directory scaffolding.
- [ ] **TASK-FE-104**: Define TypeScript data types matching ReadHead-BE Pydantic models (`src/shared/types/`).
- [ ] **TASK-FE-105**: Implement typed Axios API client with automatic JWT token refresh interceptor (`src/shared/services/apiClient.ts`).
- [ ] **TASK-FE-106**: Implement native SSE streaming reader with `fetch` and `ReadableStream` (`src/shared/services/sseStreamReader.ts`).
- [ ] **TASK-FE-107**: Implement Zustand global stores: `useAuthStore`, `useTabContextStore`, and `useSettingsStore`.
- [ ] **TASK-FE-108**: Implement client storage abstraction layer (`chrome.storage.local` + `localStorage`).

---

## Phase 2 — Chrome Extension Surfaces (Manifest V3)
- [ ] **TASK-FE-201**: Implement Background Service Worker (`background.ts`) managing tab events, alarms, and cross-context routing.
- [ ] **TASK-FE-202**: Implement Content Script text selection detector and Shadow DOM floating action menu (`SelectionWatcher`, `FloatingActionMenu`).
- [ ] **TASK-FE-203**: Implement in-page clean article extraction using `@mozilla/readability` (`DOMExtractor.ts`).
- [ ] **TASK-FE-204**: Implement in-page accessibility modifiers (OpenDyslexic font toggle, Bionic reading bolding, custom line spacing).
- [ ] **TASK-FE-205**: Implement local-first dwell time & reading pace tracker (`DwellTracker.ts`) with non-intrusive prompt chips.
- [ ] **TASK-FE-206**: Build Chrome Side Panel UI (`sidepanel.tsx`):
  - [ ] Tab header & Model selector (`Gemini`, `OpenAI`, `Claude`).
  - [ ] Chat conversation feed with markdown, syntax highlighting, and KaTeX math.
  - [ ] Real-time token streaming with animated typing cursor.
  - [ ] Grounded citation pills with host page auto-scroll trigger.
  - [ ] Thumbs up/down feedback ratings.
- [ ] **TASK-FE-207**: Implement Interactive Comprehension Quiz view (`QuizCard.tsx`) with instant answer grading and rationale display.
- [ ] **TASK-FE-208**: Build Action Popup (`popup.tsx`) for fast status inspection and one-click Side Panel toggle.
- [ ] **TASK-FE-209**: Build Options Page (`options.tsx`) for BYOK key vault management and domain blacklist configuration.

---

## Phase 3 — Privacy, Guardrails & Security
- [ ] **TASK-FE-301**: Build client-side pre-flight PII / credential detector (`sensitiveDataInspector.ts`) with default-block modal.
- [ ] **TASK-FE-302**: Implement draft preservation on session expiration (`pending_prompt_draft` storage recovery).
- [ ] **TASK-FE-303**: Implement domain blacklist check in background worker to completely silence extension on sensitive origins.
- [ ] **TASK-FE-304**: Implement zero-knowledge masked key presentation (ensure plain keys are never logged in console or stored unencrypted).

---

## Phase 4 — Web Management Portal (User & Admin)
- [ ] **TASK-FE-401**: Setup React Router DOM structure (`/login`, `/register`, `/dashboard/*`, `/admin/*`, `/docs/*`).
- [ ] **TASK-FE-402**: Build User Dashboard:
  - [ ] Reading history explorer with search and domain filtering.
  - [ ] Note and conversation export to Markdown and JSON.
  - [ ] Personal BYOK key vault manager.
  - [ ] Account profile & GDPR data purge ("Clear All History", "Delete Account").
- [ ] **TASK-FE-403**: Build Admin Management Portal:
  - [ ] System telemetry overview (live request volume, AI provider distribution pie chart, error rates).
  - [ ] User management table with pagination, search, and status toggle (Active/Suspended/Banned).
  - [ ] User satisfaction & feedback review table.
- [ ] **TASK-FE-404**: Build Static Documentation & Help Center (`/docs/*`):
  - [ ] Extension installation walkthrough.
  - [ ] BYOK guide for obtaining Google, OpenAI, and Anthropic keys.
  - [ ] Accessibility features guide & FAQ.

---

## Phase 5 — Testing, Optimization & Quality Assurance
- [ ] **TASK-FE-501**: Write Vitest unit tests for DOM extractor, sensitive text detector, and SSE reader.
- [ ] **TASK-FE-502**: Write React Testing Library tests for ChatConversation, QuizCard, and Auth forms.
- [ ] **TASK-FE-503**: Measure and optimize Content Script bundle size (< 150KB gzip).
- [ ] **TASK-FE-504**: Verify against ISO/IEC/IEEE 29148:2018 SRS Verification Matrix (`HCI (5).md` §4).
