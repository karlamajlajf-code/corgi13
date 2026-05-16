---
id: continue-20260106-033000-nonstartlive-reachability-preflight
timestamp: 2026-01-06T03:30:00Z
executed: true
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

When `tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs` is run **without** `--start-live`, fail fast with a clear, actionable hint if the required services are not already reachable (API health and web proxy health).

## Scope

- Update `tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs` only.
- Only affect the **non-`--start-live`** execution path.
- Keep existing DB status gating (exit code `2` when `/api/v1/health` JSON reports `services.database !== 'up'`).

## Required Behavior

1. **Service reachability preflight (non-`--start-live`)**

   Before running any pnpm steps (`pnpm smoke:live` / `pnpm verify:live`) or Stripe smoke:

   - If `API_HEALTH_URL` is not reachable (no 2xx response within the configured health timeout), print an actionable message and exit with code `2`.
   - If `WEB_PROXY_HEALTH_URL` is not reachable (no 2xx response within the configured health timeout), print an actionable message and exit with code `2`.

   The hint should recommend:

   - Starting services manually with `pnpm live`, **or**
   - Re-running the wrapper with `--start-live`.

   The message should include the exact URL(s) that were not reachable.

2. **Do not change start-live flow**

   `--start-live` already waits for API and proxy health; keep that behavior unchanged.

## Constraints

- No changes to `package.json` scripts.
- Keep exits deterministic and actionable; treat “prerequisites missing” as exit code `2`.

## Acceptance Criteria

- With nothing listening on ports 8003/3003, running:

  - `node ./scripts/verify-live-with-stripe-proxy-auto.mjs --skip-flow`

  exits quickly with exit code `2` and prints an actionable message pointing to the missing service(s), without running any `pnpm ...` steps.

- `--print-plan` still works.
