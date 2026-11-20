# Development Workflows — ReadHead-FE

## 1. Feature Implementation Flow
1. **Research & Requirements Traceability**:
   - Check SRS Feature (`HCI (5).md`) to identify acceptance criteria.
   - Verify corresponding backend API contract in `.docs/api/backend-contract.md`.
2. **Branching Strategy**:
   - Branch from `main`: `feat/fe-feature-name` or `fix/fe-bug-description`.
3. **Local Component Development**:
   - Create presentational components in `src/shared/components/` or surface-specific folders.
   - Use mock data first, then wire up TanStack Query hooks.
4. **Chrome API Verification**:
   - Test in unpacked Chrome Extension (`chrome://extensions/`).
   - Verify active tab switching, storage persistence, and Shadow DOM styling isolation.
5. **Testing**:
   - Write unit tests for pure functions and custom hooks.
   - Ensure `pnpm run test` and `pnpm run typecheck` pass with zero errors.
6. **Documentation Sync**:
   - If UI flows or endpoints changed, update relevant `.docs/` files.
   - Update checklist in `.docs/development/tasks.md`.
