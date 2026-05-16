---
id: continue-20260105-013000-smoke-stripe-start-api
timestamp: 2026-01-05T01:30:00Z
executed: false
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Improve the developer ergonomics of the Stripe endpoint smoke test by adding `--start-api` support (start API if not reachable, then stop it when done).

## Scope

1. Update `tejospec/scripts/smoke-stripe.mjs` to accept `--start-api`, similar to `scripts/smoke-flow.mjs`.

1. Reuse the same behavior patterns as other smoke scripts:

- Default API base: `http://localhost:8003/api/v1` (override via `API_BASE_URL`).
- Startup timeout and request timeout controlled by env vars (e.g., `SMOKE_STARTUP_TIMEOUT_MS`, `SMOKE_TIMEOUT_MS`).
- On Windows, stop process trees safely (use `taskkill`).

1. Keep behavior safe for environments without Stripe configured:

- `POST /stripe/setup-intent` returning 503 should be a **skip** by default.
- Only fail on 503 when `--require-stripe` is provided.

## Constraints

- No secrets committed.
- Keep changes minimal and consistent with existing script style.

## Acceptance Criteria

- `node tejospec/scripts/smoke-stripe.mjs --help` shows `--start-api` and env vars.
- When run with `--start-api`, the script attempts to start the API if health is not reachable and stops it at the end.
- No regression in existing Nx checks.

## Verification

- Run: `node tejospec/scripts/smoke-stripe.mjs --help`
- Run: `cd tejospec && pnpm nx run @tejo/api:lint`
