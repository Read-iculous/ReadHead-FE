---
name: api-client-integration
description: Workflow for connecting ReadHead-BE FastAPI endpoints into ReadHead-FE via TanStack Query and Zustand.
---

# API Client Integration Skill

## When to Use
Use when integrating a new or updated backend endpoint from `ReadHead-BE` into `ReadHead-FE`.

## Step-by-Step Procedure

1. **Verify Backend Contract**:
   - Check `ReadHead-BE/.docs/api/endpoints.md` and `ReadHead-FE/.docs/api/backend-contract.md`.
   - Identify the request schema and response schema.

2. **Define TypeScript Interfaces**:
   - Create or update type definition in `src/shared/types/api/`.
   - Ensure property names, optionality (`?`), and enums match Pydantic schemas exactly.

3. **Implement Service Method**:
   - Add call in `src/shared/services/` using the configured `apiClient`.

4. **Create TanStack Query Hook**:
   - In `src/shared/hooks/`, wrap the service method with `useQuery` (for GET) or `useMutation` (for POST/PUT/DELETE).
   - Set sensible `queryKey` (e.g. `['keys']`, `['conversations', page]`).
   - Implement optimistic updates or cache invalidation on mutations.

5. **Wire into UI & Handle Errors**:
   - Display loading skeleton during fetching.
   - Display actionable error toast if the request fails (e.g. Quota Exceeded, 401 Unauthorized).
