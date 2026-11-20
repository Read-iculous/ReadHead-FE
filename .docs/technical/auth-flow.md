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
Returns: { access_token, refresh_token, token_type: "bearer", expires_in: 900 }
       │
       ▼
Stored in:
- Extension: `chrome.storage.local` (encrypted session sync)
- Web Portal: `localStorage` / Secure Cookie
```

## 2. Token Lifecycle & Interceptor Logic
- **Access Token TTL**: 15 minutes.
- **Refresh Token TTL**: 7 days.
- **Axios Request Interceptor**:
  - Automatically attaches `Authorization: Bearer <access_token>` to every outbound request to ReadHead-BE.
- **Axios Response Interceptor (401 Handling)**:
  - When a 401 Unauthorized status is returned, the client locks the request queue.
  - Calls `POST /api/v1/auth/refresh` using the stored `refresh_token`.
  - If refresh succeeds: Replaces stored tokens and replays the original failed request seamlessly.
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
- Exchanges authorization code with `POST /api/v1/auth/oauth/{provider}/callback`.
