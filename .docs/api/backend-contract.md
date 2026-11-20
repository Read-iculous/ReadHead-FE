# Backend Contract — ReadHead-FE Integration

## 1. Overview
ReadHead-FE connects directly to `ReadHead-BE` (FastAPI). All requests must prefix endpoints with `/api/v1` and supply `Authorization: Bearer <access_token>` in HTTP headers (except public auth routes).

### Response Envelope Conventions
- **Standard Success**: `{"data": {...}, "message": "Optional message"}`
- **List / Paginated**: `{"data": {"items": [...], "total": 100, "page": 1, "page_size": 20, "total_pages": 5}}`
- **Error Response**: `{"detail": "Human-readable error", "code": "ERROR_CODE", "field": null}`
- **Envelope Exceptions**:
  1. `204 No Content`: Empty response body (Logout, Deletions).
  2. `GET /api/v1/health`: Raw JSON object (unwrapped) for monitoring compatibility.
  3. `POST /api/v1/ai/stream`: Server-Sent Events stream (`text/event-stream`).

---

## 2. API Endpoint Matrix

### 2.1 Authentication (`/api/v1/auth`)
| Method | Endpoint | Payload / Params | Response | Purpose |
|---|---|---|---|---|
| `POST` | `/auth/register` | `{ email, password, full_name }` | `201 Created` `{"data": { id, email, full_name, role, status, created_at }}` | New user registration |
| `POST` | `/auth/login` | `{ email, password }` | `200 OK` `{"data": { access_token, refresh_token, token_type: "bearer", expires_in: 3600 }}` | Email/Password login |
| `POST` | `/auth/oauth/google` | `{ id_token: "google_jwt_identity_token" }` | `200 OK` `{"data": { access_token, refresh_token, token_type: "bearer", expires_in: 3600 }}` | Google OAuth2 sign-in (auto-links if email exists) |
| `POST` | `/auth/oauth/github` | `{ code: "github_auth_code" }` | `200 OK` `{"data": { access_token, refresh_token, token_type: "bearer", expires_in: 3600 }}` | GitHub OAuth2 sign-in |
| `POST` | `/auth/refresh` | `{ refresh_token }` | `200 OK` `{"data": { access_token, refresh_token, token_type: "bearer", expires_in: 3600 }}` | Rotate refresh token with 30s grace period |
| `POST` | `/auth/logout` | `{ refresh_token }` | `204 No Content` | Invalidate refresh token session |

### 2.2 User Profile & Settings (`/api/v1/users`)
| Method | Endpoint | Payload / Params | Response | Purpose |
|---|---|---|---|---|
| `GET` | `/users/me` | *None* | `200 OK` `{"data": { id, email, full_name, avatar_url, role, status, is_verified, created_at }}` | Fetch current user profile |
| `DELETE`| `/users/me` | `{ confirmation: "DELETE_ACCOUNT", password }` | `204 No Content` | Delete user account & cascade personal data (`FR-HIS-07`) |
| `GET` | `/users/me/settings` | *None* | `200 OK` `{"data": UserSettingsObject}` | Get reading preferences, dyslexia font, proactive sensitivity |
| `PATCH`| `/users/me/settings` | `Partial<UserSettingsObject>` | `200 OK` `{"data": UserSettingsObject}` | Update reading & accessibility settings |

### 2.3 BYOK Key Vault (`/api/v1/api-keys` — Feature C)
| Method | Endpoint | Payload / Params | Response | Purpose |
|---|---|---|---|---|
| `GET` | `/api-keys` | *None* | `200 OK` `{"data": Array<{ provider, is_active, key_hint, last_validated_at }>}` | List stored key preview statuses |
| `POST` | `/api-keys` | `{ provider: "gemini"|"openai"|"claude", api_key: "...", set_active: true }` | `201 Created` `{"data": { provider, key_hint, is_active }}` | Upsert BYOK key (atomic replace on valid test, preserves old on failure) |
| `DELETE`| `/api-keys/{provider}` | *None* | `204 No Content` | Remove API key for specific provider |
| `POST` | `/api-keys/{provider}/validate`| *None* | `200 OK` `{"data": { valid: true, message: "Key validated" }}` | Ping-test stored key against upstream provider |

### 2.4 AI Reading Assistant & Privacy (`/api/v1/ai` — Features E, G, H)
| Method | Endpoint | Payload | Response Type | Purpose |
|---|---|---|---|---|
| `POST` | `/ai/check-sensitive` | `{ text: "..." }` | `200 OK` `{"data": { is_sensitive, detected_types, blocked_by_policy }}` | Pre-flight scan of 100% outbound text (Feature H) |
| `POST` | `/ai/stream` | `{ task: "explain"|"summarize"|"translate"|"simplify"|"chat", tab_id, page_url, page_title, selected_text?, page_content?, content_type?, user_prompt?, provider?, model?, bypass_protection? }` | `text/event-stream` (SSE: `start`, `token`, `citation`, `done`, `error`) | Unified SSE streaming endpoint with tab isolation & disconnect preservation |
| `POST` | `/ai/proactive-suggest` | `{ page_url, paragraph_text, dwell_time_seconds, bypass_protection? }` | `200 OK` `{"data": { has_suggestion, suggestion_type, hint, suggested_action }}` | Backend-assisted reading cue hint (Feature G) |
| `POST` | `/ai/feedback` | `{ message_id, rating: "helpful"|"unhelpful", report_reason?, comment? }` | `201 Created` | Rate AI response (duplicate atomically updates rating; verifies message ownership) |

### 2.5 Comprehension Quizzes (`/api/v1/quiz` — Feature F)
| Method | Endpoint | Payload / Params | Response | Purpose |
|---|---|---|---|---|
| `POST` | `/quiz/generate` | `{ page_url, page_text, bypass_protection? }` | `200 OK` `{"data": { quiz_id, questions: Array<QuizQuestion> }}` | Generate 3-5 multiple-choice questions without answers |
| `POST` | `/quiz/{quiz_id}/submit`| `{ answers: { "question_id": selected_option_index } }` | `200 OK` `{"data": { score, total, review: Array<QuestionReview> }}` | Submit answers for grading, explanation, and score logging |

### 2.6 Conversations, Reading History & Highlights (`/api/v1/conversations` & `/api/v1/reading` — Feature I)
| Method | Endpoint | Payload / Params | Response | Purpose |
|---|---|---|---|---|
| `GET` | `/conversations` | `?page=1&page_size=20&search=keyword` | `200 OK` `{"data": {"items": Array<ConversationSummary>, total}}` | Fetch paginated chat sessions |
| `GET` | `/conversations/{id}/messages`| `?page=1&limit=50&order=asc` | `200 OK` `{"data": {"items": Array<ChatMessage>}}` | Fetch paginated messages for specific conversation |
| `DELETE`| `/conversations/{id}` | *None* | `204 No Content` | Soft delete conversation & generate sync tombstone (`FR-HIS-04`) |
| `POST` | `/reading/history` | `{ page_url, page_title, reading_time_seconds, cached_summary? }` | `201 Created` | Record read article (blocked if Privacy Mode is active) |
| `GET` | `/reading/history` | `?page=1&limit=20&search=keyword` | `200 OK` `{"data": {"items": Array<HistoryItem>, total}}` | List browsing & reading history with pagination |
| `DELETE`| `/reading/history` | *None* | `204 No Content` | Clear full browsing & reading history (`FR-HIS-05`) |
| `GET` | `/reading/highlights` | `?page_url=...` | `200 OK` `{"data": {"items": Array<HighlightItem>}}` | Fetch saved highlights on web pages |
| `POST` | `/reading/highlights` | `{ page_url, highlighted_text, user_note?, color? }` | `201 Created` `{"data": HighlightItem}` | Save text highlight & personal note |

### 2.7 Multi-Device Synchronization (`/api/v1/sync` — Feature J)
| Method | Endpoint | Payload / Params | Response | Purpose |
|---|---|---|---|---|
| `POST` | `/sync/pull` | `{ cursor: "ISO_TIMESTAMP", limit: 100 }` | `200 OK` `{"data": { updated_conversations, updated_highlights, updated_settings, next_cursor, has_more }}` | Pull newer changes & tombstones since cursor |
| `POST` | `/sync/push` | `{ mutations: Array<{ entity_type, action, entity_id, base_version, payload }> }` | `200 OK` or `409 SYNC_CONFLICT` | Push local mutations with optimistic lock validation |

### 2.8 Admin Telemetry & Management (`/api/v1/admin` — Role `admin` Only)
| Method | Endpoint | Payload / Params | Response | Purpose |
|---|---|---|---|---|
| `GET` | `/admin/telemetry` | `?timeframe=24h` (`24h` \| `7d` \| `30d`) | `200 OK` `{"data": { total_requests, provider_split, error_rate, avg_latency_ms }}` | Platform telemetry & health monitoring |
| `GET` | `/admin/users` | `?page=1&limit=50&status=active` | `200 OK` `{"data": { items: Array<AdminUserView>, total }}` | View user accounts and activity |
| `PATCH`| `/admin/users/{id}/status` | `{ status: "active" \| "suspended" \| "banned" }` | `200 OK` `{"data": { user_id, new_status }}` | Moderate user account status |
| `GET` | `/admin/feedback` | `?page=1&limit=50` | `200 OK` `{"data": { items: Array<FeedbackItem>, total }}` | View user thumbs up/down and report logs |
