---
id: std-20251229-070655-continue-verify-wholesale-changes
timestamp: 2025-12-29T07:06:55Z
executed: false
originalPrompt: "Continue wholesale readiness work from the current repo state; verify recent changes compile and the smoke/verify scripts still pass; fix any regressions introduced by the latest edits."
---

# Improved Prompt

## Objective

Keep the TejoSpec wholesale readiness contract consistent and proven by running the narrowest automated checks, then fixing any regressions introduced by the latest changes.

## Scope

- Validate the current working tree compiles and passes key verification checks.
- Ensure the wholesale smoke/verify scripts and CI entrypoints remain aligned with the health contract:
  - API health: `http://localhost:8003/api/v1/health`
  - Web proxy health: `http://localhost:3003/api/health`
- Only change code/docs if a check fails or there is a concrete inconsistency.

## Constraints

- Prefer Nx-driven checks (`nx run ...`) over invoking underlying tools directly.
- Keep fixes minimal and localized; do not refactor unrelated areas.
- Do not introduce new endpoints or features unless required to fix a failing verification.

## Acceptance Criteria

- `pnpm exec nx run @tejo/api:typecheck` succeeds.
- `pnpm exec nx run @tejo/web:typecheck` succeeds.
- If Docker is available, `pnpm verify:wholesale` succeeds; otherwise, document the exact command(s) to run locally.
- No new TypeScript/lint errors introduced in changed files.

## Verification Steps (suggested order)

1. Run typechecks:
   - `pnpm exec nx run @tejo/api:typecheck`
   - `pnpm exec nx run @tejo/web:typecheck`
2. If the compose stack is available/running:
   - `pnpm verify:wholesale`
3. If any step fails, patch only what is needed and re-run the failing step.
