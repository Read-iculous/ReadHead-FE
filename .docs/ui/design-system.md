# Design System Specification — ReadHead-FE

## 1. Design Principles
- **Unobtrusive & Clean**: Never distract from reading; clean lines, generous padding, subtle drop shadows.
- **Accessible (WCAG 2.1 AA Compliant)**: High-contrast ratios, focus rings for keyboard navigation, screen-reader friendly ARIA landmarks.
- **Cognitive Ease**: Distinct typography and visual cues designed specifically for readers with ADHD, dyslexia, or cognitive fatigue.

---

## 2. Color Palette & CSS Variables

| Token | Light Mode (HSL) | Dark Mode (HSL) | Usage |
|---|---|---|---|
| `--background` | `0 0% 100%` (#FFFFFF) | `224 71% 4%` (#020817) | App canvas / background |
| `--foreground` | `224 71% 4%` (#020817) | `210 40% 98%` (#F8FAFC) | Primary text |
| `--primary` | `221.2 83.2% 53.3%` (#2563EB) | `217.2 91.2% 59.8%` (#3B82F6) | Buttons, active tabs, primary actions |
| `--primary-foreground` | `210 40% 98%` | `222.2 47.4% 11.2%` | Text on primary buttons |
| `--muted` | `210 40% 96.1%` | `217.2 32.6% 17.5%` | Card backgrounds, subtle pills |
| `--destructive` | `0 84.2% 60.2%` | `0 62.8% 30.6%` | Errors, delete actions, warnings |
| `--border` | `214.3 31.8% 91.4%` | `217.2 32.6% 17.5%` | Subtle dividers |

---

## 3. Typography & Accessibility Fonts

### 3.1 Standard Font Stack
- **Headings & UI**: `Inter`, `-apple-system`, `BlinkMacSystemFont`, `Segoe UI`, sans-serif.
- **Code & KaTeX**: `JetBrains Mono`, `Fira Code`, monospace.

### 3.2 Dyslexia & Accessibility Typography
- **OpenDyslexic Font**: Integrated for users who toggle accessibility mode. To bypass host-website Content Security Policies (CSP), `.woff2` font files are bundled directly in the extension package and loaded via `chrome-extension://<id>/assets/fonts/OpenDyslexic-Regular.woff2`.
- **Bionic Reading Fixation Cues**: Applied strictly to pure `Node.TEXT_NODE` inside article bodies while skipping interactive elements (`<a>`, `<button>`, `<code>`). Original text content is stored in a `WeakMap<Node, string>` for instant 1-click non-destructive undo.
- **Letter Spacing**: Configurable between normal (`0`), wide (`0.12em`), and extra wide (`0.2em`).
- **Line Height**: Configurable between standard (`1.5`), relaxed (`1.75`), and loose (`2.0`).

---

## 4. Component Primitives (Shadcn/ui)
All interactive elements are built using Shadcn/ui conventions:
- **Button**: Variants: `default`, `outline`, `ghost`, `destructive`, `secondary`.
- **Card**: Clean surface for quizzes, messages, and telemetry widgets.
- **Badge**: Provider indicator (`Gemini`, `OpenAI`, `Claude`) and Grounding badges (`Verified Fact`, `External Knowledge`).
- **Dialog & Drawer**: Used for sensitive data warning modals and key configuration drawers.
- **Tooltip**: Hover explanations for UI controls.
