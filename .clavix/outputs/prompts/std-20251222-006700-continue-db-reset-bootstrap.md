---
id: std-20251222-006700-continue-db-reset-bootstrap
timestamp: 2025-12-22T02:27:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Add simple, cross-platform-ish convenience commands to get a clean canonical DB + seed in one step.

## Scope

- Add pnpm scripts in `tejospec/package.json`:

  - `db:reset` (destructive: stops canonical DB and deletes its volumes, then restarts)
  - `db:bootstrap` (brings DB up, syncs schema, seeds)
  - Nx-friendly aliases: `db-reset`, `db-bootstrap`

- Update `tejospec/docs/MASTER_PLAN.md`:
  - Document the new commands under "Run Live (Local)" with clear warning for `db:reset`
  - Add completed task checkbox entries
  - Add a Change Log entry

## Constraints

- pnpm-only
- Do not break Nx project graph
- Keep changes minimal; no new tooling dependencies

## Acceptance Criteria

- `package.json` contains the new scripts
- `MASTER_PLAN.md` documents the commands
- Scripts are safe-by-default (only `db:reset` is destructive, explicitly labeled)
