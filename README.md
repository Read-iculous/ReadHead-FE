# ReadHead-FE — Intelligent Reading Assistant

Frontend client ecosystem for the **Intelligent Reading Assistant**, an AI-powered reading companion specified under ISO/IEC/IEEE 29148:2018 standards.

---

## 🚀 Overview

ReadHead-FE provides a seamless, grounded reading experience directly inside the browser:
1. **Chrome Browser Extension (Manifest V3)**:
   - **Chrome Side Panel**: Rich AI chat, Markdown & KaTeX math rendering, real-time SSE streaming, and grounded citation badges.
   - **In-Page Floating Action Menu (Shadow DOM)**: Instant Explain, Summarize, and Translate on text selection.
   - **Accessibility Re-rendering**: OpenDyslexic font injection, bionic reading fixation cues, line spacing adjustment.
   - **Comprehension Quizzes**: Dynamic multiple-choice quizzes to verify retention after reading.
   - **Local-First Proactive Assistance**: Non-intrusive prompt chips based on reading dwell time heuristics.
2. **Web Management Portal (SPA)**:
   - **User Dashboard**: Multi-device synced history, reading analytics, BYOK key vault, and data export.
   - **Admin Portal**: System health telemetry, provider distribution metrics, error tracking, and user account management.
   - **Static Help Center**: Onboarding guides, BYOK setup tutorials, and FAQs.

---

## 🛠 Tech Stack

- **Language**: TypeScript 5.0+ (Strict Mode)
- **Framework**: React 18+
- **Bundler**: Vite 5+ (with `@crxjs/vite-plugin` for Manifest V3)
- **Styling**: Tailwind CSS + Shadcn/ui (Radix UI primitives)
- **Icons**: Lucide React
- **State Management**: Zustand (Client UI State) + TanStack Query v5 (Server State)
- **AI Streaming**: Native SSE client reader (`fetch` + `ReadableStream`)
- **Storage**: `chrome.storage.local`, `chrome.storage.session`, `localStorage`

---

## 📚 Documentation Index

All technical, architectural, and operational documentation is maintained under [`.docs/`](./.docs/):

- **Project & Architecture**:
  - [Project Overview](./.docs/project-overview.md)
  - [System Context](./.docs/system-context.md)
  - [Architecture Overview](./.docs/architecture/overview.md)
  - [Component Architecture](./.docs/architecture/components.md)
  - [Architecture Decision Records (ADRs)](./.docs/architecture/decisions.md)
- **UI & UX Specifications**:
  - [Extension Surfaces (Sidepanel, Content Script, Popup, Options)](./.docs/ui/extension-surfaces.md)
  - [Web Dashboard & Admin Portal](./.docs/ui/web-dashboard.md)
  - [Design System & Accessibility](./.docs/ui/design-system.md)
- **Technical & API Contracts**:
  - [Technical Stack](./.docs/technical/tech-stack.md)
  - [Backend API Contract](./.docs/api/backend-contract.md)
  - [SSE Streaming Protocol](./.docs/api/sse-streaming.md)
  - [Authentication Flow](./.docs/technical/auth-flow.md)
  - [State Management](./.docs/technical/state-management.md)
  - [Coding Conventions](./.docs/technical/coding-conventions.md)
  - [Environment Variables](./.docs/technical/environment-variables.md)
- **Development & Tasks**:
  - [Setup Guide](./.docs/development/setup.md)
  - [Development Commands](./.docs/development/commands.md)
  - [Development Workflows](./.docs/development/workflows.md)
  - [Phased Tasks Roadmap](./.docs/development/tasks.md)
- **Knowledge Base**:
  - [Constraints & Boundaries](./.docs/knowledge/constraints.md)
  - [Glossary](./.docs/knowledge/glossary.md)
  - [Known Issues & Edge Cases](./.docs/knowledge/known-issues.md)

---

## 🤖 AI Agent Workflow

For automated coding agents (Antigravity, Cursor, Claude Code):
- Read [`AGENTS.md`](./AGENTS.md) before executing tasks.
- Adhere to the core rules in [`.agents/rules/`](./.agents/rules/).
- Follow standardized workflows in [`.agents/skills/`](./.agents/skills/).
