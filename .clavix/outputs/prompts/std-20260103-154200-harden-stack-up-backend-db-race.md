---
id: std-20260103-154200-harden-stack-up-backend-db-race
timestamp: 2026-01-03T15:42:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Harden the canonical Docker dev stack so `pnpm stack:up` is resilient and does not fail fast when the backend briefly cannot reach Postgres during startup.

## Context

- `pnpm verify:wholesale:strict` is now green end-to-end, but `pnpm stack:up` can fail because the `backend` container becomes `unhealthy` while Prisma runs migrations.
- Backend logs show transient Prisma error `P1001: Can't reach database server at db:5432` during startup.

## Scope

- Add a small, canonical-only retry mechanism for the backend’s Prisma migrate step in Docker startup.
- Keep changes localized to `tejospec/`.

## Constraints

- Do not weaken strict guardrails.
- Do not touch legacy roots (`backend/`, `frontend/`, `archive/`).
- Avoid adding new heavyweight dependencies.

## Acceptance Criteria

- `pnpm stack:up` succeeds without bailing due to backend health failures.
- Backend reaches `healthy` and `GET http://localhost:8003/api/v1/health` returns 200.
- `pnpm verify:wholesale:strict` remains green.
