---
id: std-20260103-161000-continue-verify-live
timestamp: 2026-01-03T16:10:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue to a fully verified end-state by re-running canonical runtime verification (`pnpm verify:live`) and fixing any failures introduced by recent changes, without expanding feature scope.

## Scope

- Run canonical guardrails:
  - `pnpm tejo:preflight:strict`
- Run focused build correctness checks via Nx:
  - `pnpm exec nx run @tejo/api:typecheck`
  - `pnpm exec nx run @tejo/web:typecheck`
- Run runtime verification:
  - `pnpm verify:live`

## Constraints

- Do not add new product features.
- Do not change legacy folders (`backend/`, `frontend/`, `archive/`).
- Prefer Nx tasks (`nx run ...`) over calling underlying tooling directly.

## Acceptance Criteria

- `pnpm tejo:preflight:strict` exits 0.
- `nx run @tejo/api:typecheck` and `nx run @tejo/web:typecheck` both succeed.
- `pnpm verify:live` passes (API + Web + flow smoke).
