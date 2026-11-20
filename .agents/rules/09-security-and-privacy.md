# Rule 09 — Security & Privacy Protocols

## 1. Zero-Knowledge BYOK Vault
- Frontend takes user API keys in memory only during key setup/update in the UI.
- Never write raw API keys to unencrypted `localStorage`, cookies, or browser console logs.
- Transmit directly to `ReadHead-BE` `/api/v1/keys` over HTTPS, where it is symmetrically encrypted using AES-256-GCM.
- When retrieving keys, the backend only returns masked previews (e.g. `sk-...abcd`) and active status. The frontend never possesses decrypted keys.

## 2. Client-Side Pre-Flight Sensitive Data Inspection (SRS Feature H)
- Before submitting any selected snippet or page text to `/api/v1/ai/stream`, the frontend must execute a local regex/heuristic inspection:
  - Credit card numbers, Social Security/National ID numbers.
  - Common credentials (API keys, passwords, bearer tokens, private keys).
  - Configurable user blacklisted keywords/regexes.
- **Default-Block Behavior**: If sensitive patterns are detected:
  - Display a warning dialog showing the redacted offending terms.
  - Require explicit user confirmation (`Override & Send` vs `Redact & Send` vs `Cancel`).

## 3. Local-First Proactive Monitoring (SRS Feature G)
- Reading dwell time, scroll velocity, and paragraph difficulty metrics must be computed strictly **in-memory within the local tab's Content Script**.
- Raw cursor trajectories, video streams, or user keystrokes must **never** be logged, stored, or sent across the network.
- Only a minimal trigger event (e.g. "User lingered on Section 3 for 120s with high readability index") is used to render a subtle, non-intrusive UI chip.
