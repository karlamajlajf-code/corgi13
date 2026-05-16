---
id: continue-20260111-154500-fix-smoke-live-doctor
timestamp: 2026-01-11T15:45:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Restore a default-green developer sanity-check workflow by fixing the failure in `node ./scripts/smoke-live.mjs` and/or `node ./scripts/doctor.mjs --strict` (reproduced as strict-mode warnings causing exit code 1), then re-running both scripts successfully.

## Scope

- Identify which script fails first and capture the error output.
- Fix the underlying cause (config, environment assumptions, missing files, incorrect URLs, etc.) with minimal, safe changes.
- Re-run:
  1. `node ./scripts/smoke-live.mjs`
  2. `node ./scripts/doctor.mjs --strict`

## Constraints

- Prefer repo-standard commands and Nx guidance where applicable.
- Do not introduce new backend dependencies; if the scripts require `http://localhost:3003`, ensure the dev server is started or the script handles the down state clearly.
- Keep the Playwright E2E suite behavior unchanged (it is currently green).

## Acceptance Criteria

- `node ./scripts/smoke-live.mjs` exits 0.
- `node ./scripts/doctor.mjs --strict` exits 0.
- If strict mode expects services not running by default, update the script/docs so the failure is actionable and deterministic.
