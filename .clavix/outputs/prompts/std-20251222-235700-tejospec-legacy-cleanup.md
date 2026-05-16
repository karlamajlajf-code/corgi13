---
id: std-20251222-235700-tejospec-legacy-cleanup
timestamp: 2025-12-22T23:57:00Z
executed: false
originalPrompt: "delete all not needed and old files in whole project, then start whole project and show it to me and test it"
---

# Improved Prompt

## Objective

Safely remove (or archive) legacy/unneeded folders inside `tejospec/` that are **not part of the current pnpm workspace** (which is `apps/*` and `packages/*`), without breaking the canonical dev flow. Keep the project runnable via `pnpm live` and verifiable via `pnpm typecheck`, `pnpm test`, and `pnpm db:bootstrap:flow`.

## Scope

1. Identify “legacy” directories under `tejospec/` that are **not referenced by pnpm workspace** and not required for the current runtime:
   - Candidates: `tejospec/backend/`, `tejospec/frontend/`, `tejospec/database/`, `tejospec/microservices/`, `tejospec/infrastructure/`, `tejospec/openapi/`, `tejospec/postman/`, `tejospec/k6/`, `tejospec/tests/`, `tejospec/test-results/`, `tejospec/playwright-report/`, large historical docs under `tejospec/docs/`.
2. Prefer a reversible operation first:
   - Move candidates into `tejospec/archive/legacy-YYYYMMDD/` (or similar), instead of permanent deletion.
   - Provide a dry-run report of exactly what would be moved and total size impact.
3. After cleanup, verify:
   - `pnpm install` (if needed)
   - `pnpm db:bootstrap:flow`
   - `pnpm typecheck`
   - `pnpm test`
   - `pnpm live` starts API + web.

## Constraints

- Do not delete anything inside `tejospec/apps/*` or `tejospec/packages/*`.
- Do not delete `.env*` files.
- Do not remove anything without an explicit allow-list confirmation from the user if the operation is destructive.
- If unsure whether a folder is needed, move it to archive rather than deleting.

## Acceptance Criteria

- The canonical workspace (`tejospec/apps/*`, `tejospec/packages/*`, `tejospec/scripts/*`) still builds and runs.
- `pnpm db:bootstrap:flow`, `pnpm typecheck`, and `pnpm test` succeed.
- `pnpm live` starts the API at `http://localhost:8003` and web at `http://localhost:3003`.
- A clear summary is produced listing what was archived/removed and how to restore it.
