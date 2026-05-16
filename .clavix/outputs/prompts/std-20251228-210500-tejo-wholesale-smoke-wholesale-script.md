---
id: std-20251228-210500-tejo-wholesale-smoke-wholesale-script
timestamp: 2025-12-28T21:05:00Z
executed: false
originalPrompt: "Verify the wholesale catalog-request funnel end-to-end (API + DB) and make it repeatable."
---

# Improved Prompt

## Objective

Add a **repeatable, automated smoke test** that proves the public wholesale lead funnel works end-to-end:

- API is healthy at `http://localhost:8003/api/v1/health`
- `POST /api/v1/wholesale/catalog-requests` returns `{ success: true, id }`
- The `WholesaleCatalogRequest` record is actually persisted in Postgres

## Scope

- Add a new script: `tejospec/scripts/smoke-wholesale.mjs`
- Add pnpm scripts in `tejospec/package.json`:
  - `smoke:wholesale`
  - `smoke-wholesale` (alias)

## Constraints

- Do not change existing business logic unless the smoke test uncovers a real issue.
- Keep the endpoint public (no auth).
- Keep the script cross-platform (Windows/macOS/Linux) and avoid requiring local Postgres tools.

## Acceptance Criteria

- Running `pnpm smoke:wholesale`:
  - Checks `/api/v1/health` (fails fast with actionable error message).
  - Submits a unique payload to `/api/v1/wholesale/catalog-requests`.
  - Confirms the returned `id` exists in DB (use `docker compose exec` into the backend container and query via Prisma).
  - Exits with non-zero on any failure.

## Notes

- Default API base URL: `http://localhost:8003/api/v1` (override via `API_BASE_URL`).
- Use a unique email per run to avoid collisions.
