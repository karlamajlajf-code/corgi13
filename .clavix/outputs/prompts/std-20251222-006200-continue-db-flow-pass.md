---
id: std-20251222-006200-continue-db-flow-pass
timestamp: 2025-12-22T02:25:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Get the canonical DB-backed flow smoke fully passing end-to-end on Windows.

## Scope

- Bring up canonical DB (Postgres+Redis) and ensure the API can connect without needing a restart.
- Fix `pnpm api:migrate` so it works with the current Prisma setup (Postgres schema + legacy migration history).
- Fix `pnpm api:seed` so it runs and ensures at least 1 ACTIVE product exists.
- Fix authenticated endpoints so `req.user.sub` correctly resolves to the user id.
- Run `pnpm smoke:flow` and ensure it prints `PASS: flow smoke`.
- Record all changes and the pass result in `tejospec/docs/MASTER_PLAN.md`.

## Constraints

- pnpm-only
- Keep Nx project graph stable
- Avoid breaking running dev servers

## Acceptance Criteria

- `pnpm api:migrate` succeeds (non-interactive, Windows-friendly)
- `pnpm api:seed` succeeds
- `/api/v1/health` reports `services.database = up`
- `pnpm smoke:flow` passes end-to-end
