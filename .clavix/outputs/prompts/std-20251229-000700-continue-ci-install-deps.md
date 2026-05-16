---
id: std-20251229-000700-continue-ci-install-deps
timestamp: 2025-12-29T00:07:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Make the wholesale smoke GitHub Actions workflow reliably runnable by installing pnpm dependencies before executing any `pnpm` smoke/verify scripts, while keeping the existing `api`/`proxy`/`both` behavior intact.

## Scope

- Update `.github/workflows/smoke-wholesale.yml` to add an explicit dependency install step:
  - `pnpm install --frozen-lockfile`
  - Run in the existing `tejospec` working directory (via `defaults.run.working-directory`).
- Keep existing compose startup, health waits, and run-mode branching unchanged.

## Constraints

- Minimal change: add only the missing install step.
- Don’t introduce new secrets or change ports.

## Acceptance Criteria

- Workflow YAML remains valid.
- Workflow includes a `pnpm install --frozen-lockfile` step before any `pnpm smoke:*` or `pnpm verify:*` steps.

## Verify

- Confirm `.github/workflows/smoke-wholesale.yml` has no YAML validation errors.
