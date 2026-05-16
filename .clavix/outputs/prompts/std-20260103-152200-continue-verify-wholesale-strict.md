---
id: std-20260103-152200-continue-verify-wholesale-strict
timestamp: 2026-01-03T15:22:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Finish the stabilization loop by getting canonical verification green end-to-end:

- Ensure `pnpm tejo:preflight:strict` passes from `tejospec/`.
- Bring up the canonical stack (or local dev servers) so `pnpm verify:wholesale:strict` can run and pass.

## Context

- Canonical workspace is `tejospec/` (pnpm + Nx).
- Strict preflight previously failed due to legacy diffs under `tejospec/backend/**` and `tejospec/archive/**`; those were stashed.
- Strict preflight also failed because Docker daemon wasn’t running, even though `--skip-db` was set. `scripts/doctor.mjs` was adjusted so `--skip-db` skips Docker checks.
- `pnpm verify:wholesale:strict` currently fails because the API isn’t reachable at `http://localhost:8003/api/v1/health` (stack not running).

## Scope

1. Re-run `pnpm tejo:preflight:strict` from `tejospec/` and keep it green.
2. Detect whether Docker is available/running.
3. If Docker is running: start canonical stack (`pnpm stack:up`) and wait until API health is reachable.
4. Run `pnpm verify:wholesale:strict` and fix only failures caused by our canonical work (scripts, routes, rewrites, B2B gating).

## Constraints

- Don’t lose any legacy work: keep legacy diffs stashed unless explicitly requested.
- Don’t weaken strict guardrails.
- Don’t “paper over” health failures: ensure services are actually up before claiming verification.

## Acceptance Criteria

- `pnpm tejo:preflight:strict` exits 0.
- With services running, `pnpm verify:wholesale:strict` passes in both proxy and direct API modes.

## Commands (expected)

From `tejospec/`:

- `pnpm tejo:preflight:strict`
- `docker info` (diagnostic)
- `pnpm stack:up`
- `pnpm verify:wholesale:strict`

If Docker is unavailable, provide a clear next-step message: “Start Docker Desktop, then re-run `pnpm stack:up` and `pnpm verify:wholesale:strict`.”
