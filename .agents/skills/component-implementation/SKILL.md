---
name: component-implementation
description: Step-by-step workflow for authoring accessible, typed React components using Shadcn/ui and Tailwind CSS in ReadHead-FE.
---

# Component Implementation Skill

## When to Use
Use when implementing any new UI component (whether for the Extension Side Panel, Shadow DOM Content Script, Popup, or Web Management Portal).

## Step-by-Step Procedure

1. **Verify Requirements**:
   - Check the SRS UI requirement in `ReadHead-FE/HCI (5).md` (e.g. `UI-01`, `UI-04`, `UI-06`).
   - Check `.docs/ui/design-system.md` for styling guidelines.

2. **Select or Scaffold Primitive**:
   - Check `src/shared/components/ui/` for existing Shadcn/ui primitives.
   - If a new primitive is needed, install or copy from Shadcn/ui (Radix UI).

3. **Declare TypeScript Types**:
   - Create an explicit `interface ComponentNameProps`.
   - Never use `any`.

4. **Implement Component**:
   - Write functional React component with Tailwind CSS styling.
   - Include ARIA attributes (`aria-label`, `role`, keyboard event handlers).
   - If rendering inside Content Script, ensure it mounts cleanly inside a Shadow DOM.

5. **Write Vitest / Testing Library Test**:
   - Test rendering, user click, and accessibility roles.
