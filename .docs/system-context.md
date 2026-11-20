# System Context — ReadHead-FE

## 1. System Context Diagram

```
                        ┌────────────────────────────────────────────────────────┐
                        │                   Host Web Browser                     │
                        │                                                        │
                        │   ┌──────────────────┐        ┌────────────────────┐   │
                        │   │   Host Webpage   │◄───────┤ Content Script     │   │
                        │   │   (User Reading) │ (DOM)  │ (Shadow DOM UI)    │   │
                        │   └──────────────────┘        └─────────┬──────────┘   │
                        │                                         │ (Runtime Msg)│
                        │   ┌──────────────────┐                  │              │
                        │   │  Action Popup    │                  ▼              │
                        │   └────────┬─────────┘        ┌────────────────────┐   │
                        │            │                  │ Background Worker  │   │
                        │            ▼                  │ (Service Worker)   │   │
                        │   ┌──────────────────┐        └─────────┬──────────┘   │
                        │   │ Side Panel UI    │◄─────────────────┘              │
                        │   │ (Chat/Quiz/SSE)  │                                 │
                        │   └────────┬─────────┘                                 │
                        └────────────┼───────────────────────────────────────────┘
                                     │ HTTPS REST & SSE Stream
                                     ▼
                        ┌────────────────────────────────────────────────────────┐
                        │                      ReadHead-BE                       │
                        │               (FastAPI / Python 3.10+)                 │
                        │                                                        │
                        │   ┌──────────────────┐        ┌────────────────────┐   │
                        │   │  JWT Auth & Key  │        │  Unified AI Engine │   │
                        │   │  Vault (AES-256) │        │  (SSE Streaming)   │   │
                        │   └────────┬─────────┘        └─────────┬──────────┘   │
                        └────────────┼────────────────────────────┼──────────────┘
                                     │                            │
                                     ▼                            ▼
                        ┌───────────────────────┐   ┌────────────────────────────┐
                        │ PostgreSQL + Redis DB │   │ External LLMs              │
                        │ (User, History, Sync) │   │ (Gemini, OpenAI, Claude)   │
                        └───────────────────────┘   └────────────────────────────┘
```

## 2. External Entities & System Boundaries
1. **User**: Interacts with the reading content through mouse selection, keyboard shortcuts, or opening the Side Panel.
2. **Host Web Page**: Third-party website DOM being read. Must remain completely unaffected by extension scripts (strict non-interference principle).
3. **Chrome Browser Platform**: Provides extension APIs (`chrome.sidePanel`, `chrome.storage`, `chrome.runtime`, `chrome.tabs`, `chrome.alarms`).
4. **ReadHead-BE**: The single backend gateway providing authentication, encrypted BYOK routing, history synchronization, and streaming AI responses.
5. **Web Management Portal (Browser Client)**: Standalone web client accessing ReadHead-BE for management, administration, and documentation.
