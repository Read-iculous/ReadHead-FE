# Dependencies — ReadHead-FE

## Production Dependencies (`dependencies`)

| Package | Version | Purpose |
|---|---|---|
| `react` | `^18.2.0` | Core UI engine |
| `react-dom` | `^18.2.0` | DOM rendering and Shadow DOM portals |
| `react-router-dom` | `^6.22.0` | Client routing for Web Management Portal |
| `zustand` | `^4.5.0` | Synchronous state store |
| `@tanstack/react-query` | `^5.25.0` | Asynchronous server caching & synchronization |
| `axios` | `^1.6.7` | HTTP REST client with JWT refresh interceptors |
| `lucide-react` | `^0.350.0` | Iconography suite |
| `@radix-ui/react-dialog` | `^1.0.5` | Accessible modal dialog primitives |
| `@radix-ui/react-dropdown-menu` | `^2.0.6` | Accessible menu primitives |
| `@radix-ui/react-tooltip` | `^1.0.7` | Accessible tooltip primitives |
| `@radix-ui/react-switch` | `^1.0.3` | Accessible toggle primitives |
| `@radix-ui/react-tabs` | `^1.0.4` | Accessible tabbed navigation |
| `clsx` + `tailwind-merge` | `^2.0.0` | Dynamic CSS class merging for Shadcn/ui |
| `react-markdown` | `^9.0.1` | Renders Markdown emitted by AI stream |
| `remark-gfm` | `^4.0.0` | GitHub-flavored markdown (tables, checklists) |
| `rehype-katex` + `katex` | `^7.0.0` | LaTeX mathematical equation rendering |
| `@mozilla/readability` | `^0.5.0` | Clean web article extraction from DOM |
| `zod` | `^3.22.4` | Runtime schema validation |

## Development Dependencies (`devDependencies`)

| Package | Version | Purpose |
|---|---|---|
| `typescript` | `^5.3.3` | TypeScript compiler |
| `vite` | `^5.1.4` | Fast bundler & HMR server |
| `@crxjs/vite-plugin` | `^2.0.0-beta.23`| Chrome Extension Manifest V3 bundler |
| `@types/chrome` | `^0.0.260` | Official Chrome Extension type definitions |
| `@types/react` | `^18.2.58` | React type definitions |
| `@types/react-dom` | `^18.2.19` | React DOM type definitions |
| `tailwindcss` | `^3.4.1` | Utility CSS framework |
| `postcss` + `autoprefixer`| `^8.4.35` | CSS processing pipeline |
| `vitest` | `^1.3.1` | Unit test runner |
| `@testing-library/react` | `^14.2.1` | React component testing |
| `jsdom` | `^24.0.0` | DOM environment simulation for tests |
| `eslint` | `^8.57.0` | Code quality linter |
| `prettier` | `^3.2.5` | Code formatting engine |
