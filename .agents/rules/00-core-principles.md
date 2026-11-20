# Rule 00 — Core Principles

## Purpose
Establishes the foundational mindsets and unbreakable operating laws for all engineering agents working on **ReadHead-FE**.

## Core Laws
1. **Never Assume or Hallucinate Specs**:
   - The system is formally specified under ISO/IEC/IEEE 29148:2018 (BRS, StRS, OpsCon, SyRS, SRS).
   - If an API field, UI interaction, or edge case is not found in `.docs/`, research the 6 source requirement files in `ReadHead-FE/HCI*.md` or verify against `ReadHead-BE/.docs/`. If still ambiguous, ask the user.
2. **Strict Manifest V3 Sandbox & Lifecycle Compliance**:
   - Chrome Extension Service Workers are non-persistent and can terminate at any moment. Never store crucial in-memory state in background workers without syncing to `chrome.storage.local`.
   - Never use `eval()`, `new Function()`, or inject remote executable code. All scripts must be bundled locally.
3. **Respect Content Security & User Privacy**:
   - Web pages read by the user are private. Content extraction must only read the active tab DOM on demand or under explicit user interaction.
   - PII and credentials must be filtered via client-side pre-flight checks before dispatching queries to external AI providers.
4. **Resilient UX over Silent Failures**:
   - Never let an AI streaming or network error leave the UI in an infinite loading spinner.
   - Provide clear, actionable error states (e.g. invalid BYOK key, rate limit reached, network offline) and preserve user input drafts locally so no text is lost.
