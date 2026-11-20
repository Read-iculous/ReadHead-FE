# Coding Conventions — ReadHead-FE

## 1. File Organization & Naming Rules
- **Component Files**: PascalCase, e.g. `StreamChatViewer.tsx`, `QuizCard.tsx`.
- **Custom Hooks**: camelCase starting with `use`, e.g. `useAIStream.ts`, `useDwellDetector.ts`.
- **Zustand Stores**: camelCase ending with `Store`, e.g. `authStore.ts`, `settingsStore.ts`.
- **Services & Utils**: camelCase, e.g. `apiClient.ts`, `textCleaner.ts`.
- **Types & Interfaces**: PascalCase, grouped in `src/shared/types/`.

## 2. Component Design Principles
- **Keep Components Small (< 200 lines)**: Break complex views down into smaller, focused sub-components.
- **Props Interface Export**: Always export the props interface alongside the component.
- **Accessibility by Default**: Use ARIA labels, keyboard navigability (`Enter`, `Escape`), and semantic HTML elements.
- **Controlled vs Uncontrolled**: Form inputs should be controlled or managed via `react-hook-form` + `zod`.

## 3. Asynchronous Operations & Error Boundaries
- Never leave an unhandled Promise rejection.
- Wrap UI regions in React Error Boundaries to prevent extension crash when encountering invalid third-party DOMs.
- Provide user-friendly fallback components for network failure or session expiration.

## 4. Chrome Extension Boundaries
- Never call `window.alert` or `window.prompt` inside extension content scripts (blocks host page execution).
- Clean up all event listeners (`removeEventListener`, `chrome.runtime.onMessage.removeListener`) in `useEffect` return cleanup functions to prevent memory leaks in long-running browser tabs.
