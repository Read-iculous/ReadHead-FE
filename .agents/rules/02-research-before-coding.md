# Rule 02 — Research Before Coding

## Protocol
Before writing or modifying any code in ReadHead-FE:
1. **Search Documentation First**:
   - Check `.docs/technical/tech-stack.md` and `.docs/architecture/components.md`.
   - Check `.docs/ui/extension-surfaces.md` for UI component specs.
   - Check `.docs/api/backend-contract.md` for request/response structures.
2. **Examine Existing Shared Code**:
   - Look in `src/shared/components/` before adding new UI components. Reuse Shadcn/ui primitives.
   - Look in `src/shared/stores/` before declaring redundant local React state.
   - Look in `src/shared/hooks/` for existing network, storage, or DOM hooks.
3. **Trace Requirements**:
   - Identify the requirement code from SRS (e.g. `UI-02`, `FR-AI-01`, `FR-SEC-03`).
   - Mention the requirement code in PR summaries and component JSDocs.
