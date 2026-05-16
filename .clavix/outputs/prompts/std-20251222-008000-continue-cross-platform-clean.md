---
id: std-20251222-008000-continue-cross-platform-clean
timestamp: 2025-12-22T02:40:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Make the repo easier to work with on Windows by making the root `pnpm clean` script cross-platform (no `rm -rf`).

## Scope

- Add a Node-based cleanup script at `tejospec/scripts/clean.mjs` that:
  - Deletes common build artifacts in the canonical workspace (`apps/*` and `packages/*`) and at repo root
  - Targets: `node_modules`, `dist`, `.next`, `.turbo`, `coverage`
  - Supports `--dry-run` to print what would be deleted
  - Uses only Node built-ins (no new deps)
- Update `tejospec/package.json`:
  - Replace the existing `clean` script with `node ./scripts/clean.mjs`
- Update `tejospec/docs/MASTER_PLAN.md`:
  - Add a completed checkbox item for this improvement
  - Add a Change Log entry

## Constraints

- pnpm-only
- Keep changes minimal; do not touch non-canonical legacy folders
- Do not add dependencies

## Acceptance Criteria

- `pnpm clean -- --dry-run` works on Windows and prints planned deletions
- `pnpm clean` works on Windows and removes targeted directories
- Tracking updates exist only in `tejospec/docs/MASTER_PLAN.md`
