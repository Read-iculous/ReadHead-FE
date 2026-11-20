# Technical Stack — ReadHead-FE

## 1. Core Framework & Build Tooling
| Layer | Technology | Version | Purpose |
|---|---|---|---|
| **Language** | TypeScript | `^5.3.0` | Strict type safety, shared types with backend |
| **UI Library** | React | `^18.2.0` | Declarative component architecture |
| **Bundler** | Vite | `^5.1.0` | Fast build tool and development server |
| **Extension Plugin** | `@crxjs/vite-plugin` | `^2.0.0` | Bundles Chrome Extension Manifest V3 targets |
| **Routing (Web)** | React Router DOM | `^6.22.0` | Client-side routing for Web Management Portal |

## 2. Design System & UI Styling
| Component | Library | Purpose |
|---|---|---|
| **Styling** | Tailwind CSS `^3.4.0` + PostCSS + Autoprefixer | Utility-first responsive styling |
| **UI Primitives** | Shadcn/ui (Radix UI) | Fully accessible, customizable unstyled primitives |
| **Animation** | `framer-motion` `^11.0.0` | Smooth transitions for drawers, tooltips, and chat |
| **Icons** | `lucide-react` `^0.350.0` | Consistent, accessible icon set |
| **Rich Content** | `react-markdown` + `rehype-katex` + `remark-gfm` | Markdown formatting, tables, and LaTeX math in AI output |
| **Syntax Highlighting** | `prismjs` or `shiki` | Code block formatting in AI responses |

## 3. State Management & Data Transport
| Purpose | Library | Purpose |
|---|---|---|
| **Client State** | `zustand` `^4.5.0` | Synchronous global state (active tab, UI theme, drafts) |
| **Server State** | `@tanstack/react-query` `^5.25.0` | Caching, deduplication, and sync with ReadHead-BE |
| **HTTP Client** | `axios` `^1.6.0` | Intercepted REST calls, token refresh lifecycle |
| **AI Streaming** | Native `fetch` + `ReadableStream` | Low-overhead SSE client parsing |
| **Schema Validation** | `zod` `^3.22.0` | Runtime validation of incoming API payloads |

## 4. Testing & Code Quality
| Purpose | Tool | Purpose |
|---|---|---|
| **Test Runner** | `vitest` | Ultra-fast unit & integration test runner |
| **DOM Testing** | `@testing-library/react` + `jsdom` | Component interaction and accessibility testing |
| **Chrome Mocking** | `vitest-chrome` | Mock `chrome.*` APIs for unit testing extension hooks |
| **Linter / Formatter** | ESLint + Prettier + `eslint-plugin-react-hooks` | Enforce code style and prevent memory leaks |
