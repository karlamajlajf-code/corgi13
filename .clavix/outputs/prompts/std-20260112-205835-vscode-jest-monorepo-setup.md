---
id: std-20260112-205835-vscode-jest-monorepo-setup
timestamp: 2026-01-12T20:58:35Z
executed: true
originalPrompt: "Fix VS Code Jest extension auto-detection error (multiple candidates found) in this repo and then continue."
---

# Improved Prompt

## Objective

Fix the VS Code Jest extension error:

> "Not able to auto detect a valid jest command: multiple candidates found Perhaps this is a multi-root monorepo?"

so that Jest can run/observe tests in this repository workspace without prompting for the monorepo quick-fix.

## Scope

- Workspace root: `c:\Users\patri\source\repos\corgi13`
- Configure the VS Code Jest extension explicitly for the `tejospec` monorepo (which contains multiple Jest configs) so it stops attempting auto-detection across many package roots.

## Constraints

- Prefer a deterministic, explicit configuration (`jest.projects`) over auto-detection.
- Do not change Jest test code; this is a VS Code configuration fix.
- Keep existing workspace settings intact.

## Implementation Plan

1. Update workspace settings at `c:\Users\patri\source\repos\corgi13\.vscode\settings.json`.
2. Add `jest.projects` entries that point to the known Jest roots:
   - `tejospec/backend`
   - `tejospec/apps/web`
     (Optionally add legacy `tejospec/frontend/web` later if needed.)
3. Ensure Jest remains enabled in the workspace (`jest.enable: true`).
4. Keep Jest run mode in watch mode.

## Acceptance Criteria

- The VS Code Jest extension no longer logs the auto-detection error.
- Jest shows configured projects instead of suggesting the monorepo setup quick-fix.
- Workspace settings file remains valid JSONC and retains existing settings.
