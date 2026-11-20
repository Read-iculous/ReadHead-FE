# Backend Contract — ReadHead-FE Integration

## 1. Overview
ReadHead-FE connects directly to `ReadHead-BE` (FastAPI). All requests must prefix endpoints with `/api/v1` and supply `Authorization: Bearer <access_token>` (except public auth routes).

---

## 2. API Endpoint Matrix

### 2.1 Authentication & Profile (`/api/v1/auth`)
| Method | Endpoint | Payload / Params | Response | Purpose |
|---|---|---|---|---|
| `POST` | `/auth/register` | `{ email, password, full_name }` | `{ user_id, email, access_token, refresh_token }` | New user signup |
| `POST` | `/auth/login` | `{ email, password }` | `{ access_token, refresh_token, user }` | User login |
| `POST` | `/auth/refresh` | `{ refresh_token }` | `{ access_token, refresh_token }` | Rotate expired access token |
| `GET` | `/auth/me` | *None* | `{ id, email, full_name, role, created_at }` | Get current authenticated profile |
| `POST` | `/auth/logout` | *None* | `{ message: "Logged out" }` | Invalidate refresh token |

### 2.2 BYOK Key Vault (`/api/v1/keys`)
| Method | Endpoint | Payload / Params | Response | Purpose |
|---|---|---|---|---|
| `GET` | `/keys` | *None* | `Array<{ provider, is_active, preview: "sk-...1234", last_validated }>` | List stored key statuses |
| `POST` | `/keys` | `{ provider: "gemini"|"openai"|"claude", api_key: "..." }` | `{ message: "Key stored and validated", is_valid: true }` | Save & validate BYOK key |
| `DELETE` | `/keys/{provider}`| *None* | `{ message: "Key deleted" }` | Remove user key |

### 2.3 AI Reading Assistance (`/api/v1/ai`)
| Method | Endpoint | Payload | Response Type | Purpose |
|---|---|---|---|---|
| `POST` | `/ai/stream` | `{ task: "explain"|"summarize"|"translate"|"qa", page_url, page_title, selected_text?, full_page_context?, conversation_id?, provider? }` | `text/event-stream` (SSE) | Streamed AI reading response with citations |
| `POST` | `/ai/quiz` | `{ page_url, page_title, page_content, num_questions: 3 }` | `{ questions: Array<QuizQuestion> }` | Generate comprehension quiz |
| `POST` | `/ai/feedback`| `{ message_id, rating: "helpful"|"unhelpful", reason_tag?, comment? }` | `{ status: "ok" }` | Log user feedback |

### 2.4 History & Multi-Device Sync (`/api/v1/history` & `/api/v1/sync`)
| Method | Endpoint | Payload / Params | Response | Purpose |
|---|---|---|---|---|
| `GET` | `/history/conversations` | `?page=1&limit=20` | `{ items: Array<ConversationSummary>, total }` | Fetch conversation history |
| `GET` | `/history/conversations/{id}` | *None* | `{ conversation: DetailedConversation }` | Fetch specific conversation messages |
| `DELETE`| `/history/conversations/{id}` | *None* | `{ message: "Deleted" }` | Delete single conversation |
| `DELETE`| `/history/all` | *None* | `{ message: "All history cleared" }` | Clear full user history |
| `POST` | `/sync/pull` | `{ last_synced_at }` | `{ updates: Array<SyncUpdateItem>, timestamp }` | Pull newer updates from cloud |
| `POST` | `/sync/push` | `{ changes: Array<SyncItem> }` | `{ status: "synced", conflicted_ids: [] }` | Push local offline changes |

### 2.5 Admin Telemetry & Management (`/api/v1/admin`) — Role `Admin` Only
| Method | Endpoint | Payload / Params | Response | Purpose |
|---|---|---|---|---|
| `GET` | `/admin/telemetry` | `?timeframe=24h` | `{ total_requests, provider_split, error_rate, avg_latency }` | Platform health monitoring |
| `GET` | `/admin/users` | `?page=1&limit=50&status=active` | `{ users: Array<AdminUserView>, total }` | View user accounts |
| `PATCH`| `/admin/users/{id}/status` | `{ status: "active"|"suspended"|"banned" }` | `{ user_id, new_status }` | Ban/Suspend user account |
| `GET` | `/admin/feedback` | `?page=1&limit=50` | `{ feedback_items: Array<FeedbackItem> }` | View user feedback logs |
