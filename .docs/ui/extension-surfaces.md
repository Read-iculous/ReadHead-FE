# Extension Surfaces Specification — ReadHead-FE

## 1. Extension Surfaces Overview

The Chrome Extension delivers functionality across 4 distinct user interfaces:

```
┌────────────────────────────────────────────────────────────────────────────┐
│ 1. IN-PAGE CONTENT SCRIPT (Host Webpage)                                   │
│    - Text selection creates floating quick-action bubble (Shadow DOM)      │
│    - Injects OpenDyslexic / Bionic Reading cues into host page body        │
│    - Subtle proactive prompt chip in bottom corner when reader is stuck    │
├────────────────────────────────────────────────────────────────────────────┤
│ 2. CHROME SIDE PANEL (Primary Workspace)                                   │
│    - Embedded alongside the webpage; never overlaps or covers text         │
│    - Full AI chat conversation, SSE streaming response, KaTeX rendering    │
│    - Interactive Comprehension Quiz cards & evaluation feedback            │
├────────────────────────────────────────────────────────────────────────────┤
│ 3. ACTION POPUP (Toolbar Click)                                            │
│    - Quick glance: Connection status, active AI provider                   │
│    - Master toggles: Extension On/Off, Accessibility Mode On/Off           │
│    - One-click button: "Open Side Panel"                                  │
├────────────────────────────────────────────────────────────────────────────┤
│ 4. OPTIONS PAGE (Dedicated Full-Tab Settings)                              │
│    - BYOK Key Vault manager (Google Gemini, OpenAI, Anthropic Claude)      │
│    - Domain blacklist & whitelist manager                                  │
│    - Default reading preferences & proactive dwell threshold tuning        │
└────────────────────────────────────────────────────────────────────────────┘
```

---

## 2. In-Page Content Script & Shadow DOM Overlay

### 2.1 Floating Action Bubble (Text Selection)
- **Trigger**: User highlights text (`window.getSelection()`).
- **Placement**: Floats directly above the selected text with collision detection (clamps to window edges).
- **Actions**:
  - `Explain`: Sends snippet to Side Panel with task prompt "Explain this simply".
  - `Summarize`: Bullet-point summary of highlighted passage.
  - `Translate`: Translates into user's preferred language.
  - `Ask AI`: Opens Side Panel with text prepended as context.
- **Shadow DOM Isolation**:
  ```ts
  const container = document.createElement('readhead-overlay-root');
  const shadow = container.attachShadow({ mode: 'closed' });
  // Mount React component tree inside shadow
  ```

### 2.2 In-Page Accessibility Re-Rendering (SRS BG-06)
- **OpenDyslexic Font Toggle**: Injects font-family `@font-face` and applies clean letter-spacing and word-spacing to article paragraphs without breaking layout.
- **Bionic Reading Fixation**:
  - Algorithm bolds the first 30%-50% of each word in the target paragraph.
  - Example: `**Int**elligent **Read**ing **Ass**istant`.
  - Non-destructive: Operates on cloned nodes or spans; original text nodes can be toggled back immediately.

---

## 3. Chrome Side Panel UI (Primary Workspace)

### 3.1 Layout Anatomy
- **Header**: Active tab domain badge, model selector (`gemini-1.5-flash`, `gpt-4o-mini`, etc.), pin/clear conversation button.
- **Message Feed**:
  - User messages (right-aligned).
  - AI responses (left-aligned) with provider logo, live token cursor during streaming, copy button, and thumbs up/down rating buttons.
  - Source Citation Pills (e.g. `[Source: §3.2]`) which, when clicked, scroll the host webpage to the referenced paragraph.
- **Input Area**: Auto-resizing textarea, "Ask about page" pill, send button, draft recovery banner if session previously expired.

### 3.2 Interactive Quiz View (Feature F)
- When user completes a long article, or clicks "Test my comprehension":
  - Renders a multi-choice quiz card (3-5 questions generated from page content).
  - User selects an answer: Card immediately indicates Correct/Incorrect with color feedback and displays an in-depth rationale grounded in the text.
