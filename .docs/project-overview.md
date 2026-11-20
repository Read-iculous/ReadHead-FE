# Project Overview — Intelligent Reading Assistant (ReadHead-FE)

## 1. Executive Summary
**ReadHead-FE** is the comprehensive frontend client ecosystem for the **Intelligent Reading Assistant**. Built according to ISO/IEC/IEEE 29148:2018 standards and verified against the 6 core specification documents (BRS, StRS, OpsCon, SyRS, RTM, and SRS Draft 0.1), it provides an unobtrusive, accessible, and grounded reading interface directly inside modern web workflows.

The frontend is architected as a dual-surface solution:
1. **Chrome Browser Extension (Manifest V3)**: The primary daily-driver client operating seamlessly alongside web reading materials via Chrome Side Panel, Content Script overlays (Shadow DOM), and Action Popups.
2. **Web Management Portal (Dashboard & Docs)**: A standalone web application delivering user-level configuration & synced reading analytics, enterprise/admin platform health monitoring, and a static onboarding help site.

---

## 2. Business Goals (BRS §3.1.1)
- **BG-01 — Reduce Manual Reading Effort**: Eliminate context-switching between reading tabs and translation/lookup tools.
- **BG-02 — Improve Content Comprehension**: In-context explanation of domain terminology, complex idioms, and multi-lingual translation.
- **BG-03 — Knowledge Retention**: Reinforce learning through auto-generated comprehension quizzes and key takeaway syntheses.
- **BG-04 — Reading Continuity**: Multi-device synchronization of reading state, chat history, and highlights.
- **BG-05 — Personalization**: Tailored explanation styles, custom reading pace, and Bring-Your-Own-Key (BYOK) provider routing.
- **BG-06 — Reading Accessibility**: Custom typography, OpenDyslexic font support, bionic reading fixation cues, and AI text simplification.
- **BG-07 — Control, Transparency & Trust**: Strict client-side PII inspection, transparent AI citations, and AES-256 encrypted credential handling.
- **BG-08 — Measurable Evaluation**: Instant feedback loops (helpful/unhelpful ratings) and anonymized operational telemetry.

---

## 3. Product Scope: 12 SRS Features + Admin Management

### Core Client Features (SRS §3.2)
| Feature Code | Feature Name | Frontend Scope & Responsibilities |
|---|---|---|
| **Feature A** | **Account & Session Management** | Login/Register forms, Google/GitHub OAuth2 popup handshake, auto-refresh JWT cycle, draft preservation on session expiry. |
| **Feature B** | **Onboarding & Configuration** | First-run interactive setup wizard, default AI model selection, proactive assistance toggle, privacy mode setting. |
| **Feature C** | **API Key Vault (BYOK)** | Secure in-memory key input, pre-flight key validation via BE, masked status indicators (`sk-...abcd`), error isolation. |
| **Feature D** | **Content Extraction & Context** | Active tab DOM article parsing (Readability/DOMParser), text selection capture, per-tab context isolation. |
| **Feature E** | **AI Reading Assistance** | Real-time SSE token streaming, markdown/KaTeX math rendering, citations highlight, on-page vs external knowledge badge. |
| **Feature F** | **Comprehension Questions** | Interactive multiple-choice quiz cards triggered after reading milestones; instant feedback and explanation reveal. |
| **Feature G** | **Proactive Reading Support** | Local-first lightweight dwell time & text density monitoring; non-intrusive floating suggestion chips. |
| **Feature H** | **Privacy & Data Protection** | Client-side pre-flight PII/credential detector with default-block modal, domain blacklist/whitelist toggle. |
| **Feature I** | **Conversation History & Lifecycle** | Per-tab chat history viewer, granular conversation deletion, full history wipe, local draft recovery. |
| **Feature J** | **Sync & Conflict Handling** | Multi-device sync indicators, offline indicator, local fallback caching when connection drops. |
| **Feature K** | **Error Handling & Recovery** | Distinguish between key/quota errors, network offline, and AI timeouts; local draft preservation; manual retry. |
| **Feature L** | **Monitoring & Operational Support** | User feedback rating buttons (thumbs up/down with reason tag), client performance metrics. |

### Extended Platform Scope: Web Management Portal (Admin & User)
- **User Dashboard**: Review cross-device reading stats, manage saved highlights, export reading notes (Markdown/JSON), manage BYOK keys.
- **Admin Management Portal**:
  - *User Management*: Paginated user list, account status toggle (Active / Suspended / Banned), role management (User / Admin).
  - *System Telemetry & Health*: Live overview of total requests, upstream AI provider distribution (OpenAI vs Gemini vs Claude), error rates (4xx, 5xx, QuotaExceeded), and user feedback satisfaction score.
  - *Privacy Guarantee*: Admin has zero access to user decrypted API keys, reading text, or private conversation logs.
- **Static Documentation & Help Center**: Comprehensive user guides, keyboard shortcuts, Chrome Extension installation walkthrough, and troubleshooting handbook.
