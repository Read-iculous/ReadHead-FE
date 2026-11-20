# Rule 07 — React & Vite Conventions

## 1. Directory Structure Conventions
- Component files: PascalCase (e.g. `ChatStreamViewer.tsx`, `QuizModal.tsx`).
- Hooks files: camelCase prefixed with `use` (e.g. `useAIStream.ts`, `useDwellTracker.ts`).
- Service / Store / Utility files: camelCase (e.g. `apiClient.ts`, `authStore.ts`, `domExtractor.ts`).
- Route pages: PascalCase or lowercase index files within route folder (e.g. `pages/admin/Telemetry.tsx`).

## 2. Vite Build & Aliasing
- Use Path Aliases defined in `tsconfig.json` and `vite.config.ts`:
  - `@/` -> `src/`
  - `@shared/` -> `src/shared/`
  - `@extension/` -> `src/extension/`
  - `@web/` -> `src/web/`
- Avoid deeply nested relative imports (e.g. `../../../../shared`).

## 3. Performance & Bundle Splitting
- Sidepanel and Web Portal must leverage dynamic code splitting (`React.lazy`, `Suspense`) for heavy views (e.g. Markdown renderer, Katex math parser, Quiz analytics charts).
- Ensure Content Script bundle is lean (< 150KB gzip) to avoid injecting latency into host web pages.
