# Web Dashboard & Help Center Specification — ReadHead-FE

## 1. Web Portal Scope
The Web Management Portal complements the Chrome Extension, giving users and platform administrators a spacious, desktop-grade environment for managing settings, reviewing synchronized reading history, and monitoring system health.

```
┌────────────────────────────────────────────────────────────────────────────┐
│ WEB MANAGEMENT PORTAL (Vite SPA)                                           │
├──────────────────────────────┬─────────────────────────────┬───────────────┤
│ 1. User Dashboard            │ 2. Admin Portal             │ 3. Static Help│
│ - Reading History & Notes    │ - System Health & Telemetry │ - Quick Start │
│ - Comprehension Analytics    │ - User Account Management   │ - BYOK Guides │
│ - Cloud Sync & Export        │ - AI Provider Breakdown     │ - FAQs & Tips │
│ - BYOK Vault Management      │ - Error & Satisfaction Log  │ - Shortcuts   │
└──────────────────────────────┴─────────────────────────────┴───────────────┘
```

---

## 2. User Dashboard Views (`/dashboard/*`)

### 2.1 Reading Analytics & History (`/dashboard/history`)
- **Searchable Reading Archive**: View all articles read across devices where ReadHead was used.
- **Filters**: By Date, Domain, Provider used, or Tag.
- **Export Capabilities**: Export notes, summaries, and chat transcripts to Markdown (`.md`), PDF, or JSON.
- **Comprehension Progress**: Visual score tracker charting quiz performance over time.

### 2.2 Account & Key Vault (`/dashboard/settings`)
- **BYOK Key Manager**: Add or rotate API keys for Google Gemini, OpenAI, and Anthropic.
- **Reading Profiles**: Manage pre-set explanation levels (e.g. *ELI5 / Beginner*, *Academic / Researcher*, *Executive Summary*).
- **GDPR / Privacy Center**: "Delete All History", "Export All Personal Data", and "Delete My Account" with one-click verification.

---

## 3. Admin Management Portal (`/admin/*`)

### 3.1 System Telemetry & Operational Health (`/admin/telemetry`)
- **Request Volume**: Total AI requests served (last 24 hours / 7 days / 30 days).
- **Provider Distribution**: Pie/Bar chart of provider usage (e.g. 55% Gemini, 30% OpenAI, 15% Claude).
- **Error Breakdown**: Real-time error rate tracking (Rate Limit 429, Invalid Key 401, Upstream Timeout 504).
- **Zero-Knowledge Privacy**: Admin views aggregate numeric counters only; no personal user text or prompt content is ever visible to admins.

### 3.2 User Management (`/admin/users`)
- **User Table**: ID, Email, Role (`User` / `Admin`), Registration Date, Last Active, Status (`Active` / `Suspended` / `Banned`).
- **Actions**: Toggle status, reset password link trigger, revoke active sessions.

### 3.3 User Feedback & Quality Metrics (`/admin/feedback`)
- Aggregated thumbs-up / thumbs-down satisfaction percentage.
- Categorized problem feedback (e.g. "Hallucination/Ungrounded", "Too verbose", "Formatting issue").

---

## 4. Static Help Center & Documentation (`/docs/*`)
- **Installation Walkthrough**: Step-by-step instructions for loading the extension unpacked (or from Chrome Web Store).
- **BYOK Setup Guide**: How to obtain free API keys from Google AI Studio, OpenAI Platform, and Anthropic Console.
- **Troubleshooting Handbook**: Common network errors, permissions explanations, and privacy FAQ.
