---
id: std-20251222-023200-continue-doctor-strict-negative-db
timestamp: 2025-12-22T02:32:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Harden and document strict-mode diagnostics by adding safe, pnpm-first DB stop/start commands and verifying that `pnpm tejo:doctor:strict` exits non-zero when the canonical DB containers are stopped.

## Scope

- Add non-destructive canonical DB lifecycle scripts:
  - `pnpm db:stop` / `pnpm db-stop` → `docker compose -f docker-compose.canonical-db.yml stop`
  - `pnpm db:start` / `pnpm db-start` → `docker compose -f docker-compose.canonical-db.yml start`
- Update `tejospec/docs/MASTER_PLAN.md`:
  - Document strict-mode semantics (non-zero on any `WARN`).
  - Add a safe repro for DB-related strict failure using `pnpm db:stop` then `pnpm tejo:doctor:strict`, then `pnpm db:start`.
  - Add a completed task checkbox entry + changelog note.
- Verify behavior locally:
  - With DB healthy: `pnpm tejo:doctor:strict` exits 0.
  - After `pnpm db:stop`: `pnpm tejo:doctor:strict` exits 1 and shows DB service warnings.
  - After `pnpm db:start`: strict returns to green.

## Constraints

- pnpm-only.
- No new dependencies.
- Do not delete volumes/data (no `down -v`); use stop/start.
- Keep all tracking in `tejospec/docs/MASTER_PLAN.md`.

## Acceptance Criteria

- New scripts exist in `tejospec/package.json`: `db:stop`, `db:start`, and Nx-friendly aliases `db-stop`, `db-start`.
- `pnpm tejo:doctor:strict` returns non-zero when DB is stopped.
- `MASTER_PLAN.md` documents the safe strict-mode negative test and includes a changelog entry.
