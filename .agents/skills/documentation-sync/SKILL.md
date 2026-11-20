---
name: documentation-sync
description: Workflow for keeping ReadHead-FE documentation (.docs/) perfectly aligned with codebase changes.
---

# Documentation Sync Skill

## When to Use
Use immediately whenever UI architecture, dependencies, API contracts, environment variables, or development tasks are created or modified.

## Step-by-Step Procedure

1. **Audit Changes**:
   - Run `git status` or review changed files.
2. **Identify Impacted Documentation**:
   - If UI components or surfaces changed -> update `.docs/ui/extension-surfaces.md` or `.docs/ui/web-dashboard.md`.
   - If API endpoints or payloads changed -> update `.docs/api/backend-contract.md`.
   - If packages or scripts changed -> update `.docs/technical/dependencies.md` or `.docs/development/commands.md`.
   - If a phase task was finished -> mark `[x]` in `.docs/development/tasks.md`.
3. **Validate Consistency**:
   - Ensure terms and file paths match `AGENTS.md` and `ReadHead-FE/.docs/`.
