---
id: std-20251222-000000-tejospec-cleanup-run
timestamp: 2025-12-22T00:00:00Z
executed: false
originalPrompt: "delete all not needed and old files in whole project, then start whole project and show it to me and test it"
---

# Improved Prompt (Implementation-Ready)

## Objective

Safely clean up the repository by removing clearly generated/temporary artifacts and consolidating obvious legacy “old/backups” without deleting important source code, then successfully start and validate the canonical project.

## Scope

- **Canonical workspace**: `tejospec/` (pnpm + Nx monorepo; apps: `apps/web` and `apps/api`).
- **Cleanup**:
  1. Run built-in cleanup in **dry-run** mode first.
  2. Produce a human-readable list of proposed deletions (grouped by “generated artifacts”, “reports”, “caches”, “legacy backups”).
  3. Only after confirmation, perform the real cleanup.
- **Start**:
  - Bootstrap env templates, start database stack (Docker), then run combined dev via the repo’s recommended scripts.
- **Test**:
  - Run the repo’s doctor/smoke checks and targeted automated checks (typecheck + tests). Optionally run Playwright e2e if environment supports it.

## Constraints / Safety Rules

- Do **not** delete or rewrite application source without listing it first.
- Prefer the repo’s canonical commands:
  - `pnpm clean:dry-run` → propose cleanup
  - `pnpm clean` → execute cleanup (only after confirmation)
  - `pnpm env:bootstrap` → safe env creation (no overwrite)
  - `pnpm db:bootstrap:flow` → docker up + migrate + seed + smoke flow
  - `pnpm live` → start web+api together
- Treat folders explicitly marked “Legacy / Historical” as **out of scope** unless user confirms they can be removed.

## Acceptance Criteria

- Provide a cleanup proposal report (dry-run results) before any destructive deletion.
- Project installs and boots successfully from `tejospec/`.
- App URLs are reachable:
  - Web: <http://localhost:3003>
  - API: <http://localhost:8003> (health endpoint reachable)
- Validation passes:
  - `pnpm tejo:doctor` succeeds (or strict mode, if feasible)
  - `pnpm typecheck` succeeds
  - `pnpm test` succeeds (and `pnpm test:e2e` if supported)

## Notes

- If Docker is unavailable, skip DB bootstrap and run non-DB validations; clearly report the blocker and required prerequisites.
