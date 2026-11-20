# Rule 04 — Coding Standards

## 1. Language & Typing
- **TypeScript 5.0+ Strict Mode**: `noImplicitAny: true`, `strictNullChecks: true`.
- **Never use `any`**: Use `unknown` with type guards, or define explicit TypeScript `interface` / `type`.
- Types matching backend models must be grouped under `src/shared/types/api/`.

## 2. React Best Practices
- **Function Components Only**: No class components.
- **Explicit Props Typing**: Declare interfaces for all component props.
- **Custom Hooks for Logic**: Complex `useEffect` logic or multi-step async operations must be extracted into descriptive custom hooks (`useReadingSelection`, `useAIStream`, `useAuth`).
- **Clean Component File Structure**:
  ```tsx
  // 1. Imports (external -> internal shared -> relative)
  import { useState } from 'react';
  import { Button } from '@/shared/components/ui/button';
  
  // 2. Types/Interfaces
  interface ChatInputProps {
    onSend: (message: string) => void;
    disabled?: boolean;
  }
  
  // 3. Component Definition
  export const ChatInput: React.FC<ChatInputProps> = ({ onSend, disabled }) => {
    ...
  };
  ```

## 3. Styling & Design System
- Utilize **Tailwind CSS** utility classes directly.
- Component primitives must leverage **Shadcn/ui** (based on Radix UI) to guarantee WAI-ARIA accessibility.
- Support **Dark Mode** and **High Contrast / Dyslexia** color schemes out of the box using CSS variables.
