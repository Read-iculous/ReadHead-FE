# Rule 05 — Testing and Validation

## 1. Unit & Component Testing
- **Test Runner**: Vitest + React Testing Library.
- Every shared utility in `src/shared/utils/` (e.g. sensitive text detector, DOM parser) must have 100% unit test coverage.
- Component tests must verify accessible roles and user interactions, not internal state details.

## 2. Chrome API Mocking
- Extension components and hooks must be tested using `@types/chrome` and mock fixtures (`jest-chrome` or custom Vitest mocks for `chrome.runtime`, `chrome.tabs`, `chrome.storage`).
- Components must gracefully degrade or show fallbacks when rendered outside an extension environment (e.g., during web preview or Storybook testing).

## 3. Streaming & Network Testing
- Mock SSE streaming using `ReadableStream` generators to test:
  - Normal chunk-by-chunk token emission.
  - Abrupt connection drop and reconnect.
  - Upstream error packet parsing (`error_code`, `detail`).
