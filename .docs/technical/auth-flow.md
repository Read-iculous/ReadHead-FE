# Authentication Flow — ReadHead-FE

## 1. Overview
ReadHead-FE integrates with ReadHead-BE using a **Dual-Token JWT Architecture (Access + Refresh)** supplemented with OAuth2 (Google & GitHub).

```
User Action (Login) 
       │
       ▼
POST /api/v1/auth/login ──────────► ReadHead-BE
       │                                  │
       ◄──────────────────────────────────┘
Returns: { "data": { access_token, refresh_token, token_type: "bearer" } }
       │
       ▼
Stored in:
- Extension: `chrome.storage.local` (encrypted session sync)
- Web Portal: `localStorage` / Secure Cookie
```

## 2. Token Lifecycle & Interceptor Logic
- **Access Token TTL**: 60 minutes.
- **Refresh Token TTL**: 30 days.
- **Axios Request Interceptor**:
  - Automatically attaches `Authorization: Bearer <access_token>` to every outbound request to ReadHead-BE.
- **Axios Response Interceptor (401 Handling)**:
  - When a 401 Unauthorized status is returned with code `TOKEN_EXPIRED`, the client locks the request queue.
  - Calls `POST /api/v1/auth/refresh` using the stored `refresh_token`.
  - If refresh succeeds: Replaces stored `access_token` and replays the original failed request seamlessly.
  - If refresh fails (token revoked or expired): Fires `AUTH_EXPIRED` event to `useAuthStore`.

## 3. Draft Preservation on Expiry (SRS FR-ACC-06)
- When a session expires during active reading or composition:
  - The current in-progress question or prompt is **never discarded**.
  - It is serialized to `chrome.storage.local` under `pending_prompt_draft`.
  - The UI presents a lightweight re-authentication modal.
  - Upon successful login, the draft is restored to the input box with an indicator.

## 4. OAuth2 Popup Flow for Chrome Extension
- Extension invokes `chrome.identity.launchWebAuthFlow`.
- Redirect URL: `https://<extension-id>.chromiumapp.org/oauth2`.
- Exchanges token / authorization code with `POST /api/v1/auth/oauth/google` or `POST /api/v1/auth/oauth/github`.

## 5. Multi-Account Isolation & Storage Purge Protocol (`FR-ACC-04`)
To prevent cross-account data leaks when switching user accounts on the same browser:
1. **Local Storage Wipe**: On `logout()` or account switch, the client immediately removes `access_token`, `refresh_token`, all `draft_*` keys, and `cached_article_context` from `chrome.storage.local` and `sessionStorage`.
2. **Context Broadcast**: The Background Service Worker broadcasts an internal runtime message `ACTION_PURGE_SESSION` to all open Side Panels, Popups, and Content Scripts.
3. **State Invalidation**: All Zustand global stores (`useAuthStore`, `useTabContextStore`, `useSettingsStore`) are reset to initial empty states, and in-flight fetch requests are cancelled via `AbortController`.
4. **UI Redirect**: Open extension panels redirect immediately to the `/login` view.
