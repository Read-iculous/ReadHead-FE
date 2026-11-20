# Rule 01 — Project Awareness

## Ecosystem Context
**ReadHead-FE** is half of a dual-repository solution:
- **ReadHead-BE**: FastAPI backend handling JWT authentication, PostgreSQL data persistence, Redis caching, AES-256 BYOK encryption, and SSE-based unified AI streaming across OpenAI, Gemini, and Claude.
- **ReadHead-FE**: React + TypeScript client providing the Chrome Extension interface (Manifest V3 Side Panel, Content Tooltips, Options) and the Web Management Portal (User Dashboard, Admin Monitoring, Static Help).

## Key Source Files Reference
- `ReadHead-FE/HCI.md` — Business Requirements Specification (BRS)
- `ReadHead-FE/HCI (1).md` — Operational Concept Description (OpsCon)
- `ReadHead-FE/HCI (2).md` — Stakeholder Requirements Specification (StRS)
- `ReadHead-FE/HCI (3).md` — System Requirements Specification (SyRS)
- `ReadHead-FE/HCI (4).md` — Requirements Traceability Matrix (RTM) & Review Notes
- `ReadHead-FE/HCI (5).md` — Software Requirements Specification (SRS Draft 0.1)
- `ReadHead-BE/.docs/api/endpoints.md` — Backend API Contract & Schema Definitions

## Golden Rule of Project Awareness
Before creating a new feature or modifying an existing flow:
1. Verify which SRS Feature (Feature A to L) owns this capability.
2. Verify the corresponding Backend endpoint in `ReadHead-BE/.docs/api/endpoints.md`.
3. Ensure FE data models exactly mirror BE Pydantic models.
