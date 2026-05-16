---
id: std-20251222-007300-continue-db-bootstrap-flow
timestamp: 2025-12-22T02:33:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Add a single command to bootstrap the canonical DB and immediately verify the DB-backed happy path flow smoke.

## Scope

- Add pnpm scripts in `tejospec/package.json`:
  - `db:bootstrap:flow`: runs `db:bootstrap` then `smoke:flow`
  - Nx-friendly alias `db-bootstrap-flow`
- Update `tejospec/docs/MASTER_PLAN.md`:
  - Document the new command under "Run Live (Local)"
  - Add a completed checkbox entry
  - Add a Change Log line

## Constraints

- pnpm-only
- Keep changes minimal; no new dependencies
- Do not add new plan files; only update `tejospec/docs/MASTER_PLAN.md`

## Acceptance Criteria

- Running `pnpm db:bootstrap:flow` executes DB up → schema sync → seed → flow smoke
- `pnpm exec nx run @tejo/root:db-bootstrap-flow` works as an alias
- `MASTER_PLAN.md` is updated and remains markdownlint-clean
