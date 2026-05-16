---
id: std-20260103-153500-finish-wholesale-strict-runtime
timestamp: 2026-01-03T15:35:00Z
executed: false
originalPrompt: "(context drop) Finish the loop: bring stack up + run verify:wholesale:strict"
---

# Improved Prompt

## Objective

Complete the “to the very end” stabilization loop for the canonical `tejospec/` workspace:

- Keep `pnpm tejo:preflight:strict` green.
- Start the runtime stack so API/Web endpoints are reachable.
- Run `pnpm verify:wholesale:strict` end-to-end and make it pass.

## Constraints

- Canonical workspace is `tejospec/` (pnpm + Nx). Avoid touching legacy roots (`backend/`, `frontend/`, `archive/`) unless explicitly required.
- Don’t weaken strict guardrails.
- If Docker isn’t running, don’t “fix” code to accommodate; provide actionable steps.

## Plan

From `tejospec/`:

1. Run `pnpm tejo:preflight:strict`.
2. Check Docker daemon (`docker info`).
3. If Docker is available: `pnpm stack:up` and wait for health:
   - `http://localhost:8003/api/v1/health`
   - `http://localhost:3003/api/health`
4. Run `pnpm verify:wholesale:strict`.
5. If failures occur, fix only canonical scripts/config/routes and rerun verification.

## Acceptance Criteria

- `pnpm tejo:preflight:strict` exits 0.
- `pnpm verify:wholesale:strict` passes with the stack running.
