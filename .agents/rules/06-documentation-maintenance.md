# Rule 06 — Documentation Maintenance

## Obligation to Synchronize
Documentation in `.docs/` is the single source of truth for all human developers and AI agents.
Any code change that alters:
1. An API payload or response field -> Update `.docs/api/backend-contract.md`.
2. A new UI component or surface behavior -> Update `.docs/ui/extension-surfaces.md` or `.docs/ui/web-dashboard.md`.
3. An environment variable -> Update `.docs/technical/environment-variables.md`.
4. A completed development task -> Check off the item in `.docs/development/tasks.md`.

## Strict Non-Guessing Policy
If code and documentation conflict, verify with `ReadHead-FE/HCI (5).md` (SRS) or stop and query the user.
