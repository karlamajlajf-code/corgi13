---
id: continue-20260105-025000-smoke-stripe-start-web-api-hint
timestamp: 2026-01-05T02:50:00Z
executed: true
originalPrompt: "continue working!"
---

# Improved Prompt

## Objective

Improve `--start-web` ergonomics in the Stripe smoke script by failing fast with a clear hint when proxy mode depends on an API that isn’t reachable.

## Scope

1. Update [tejospec/scripts/smoke-stripe.mjs](tejospec/scripts/smoke-stripe.mjs):

- In `ensureWebRunningIfRequested()`, when `--proxy` + `--start-web` is used **without** `--start-api` and the direct API health (`API_HEALTH_URL`) is not reachable, fail fast with a message explaining that proxy health depends on the API and to start API or pass `--start-api`.
- Do not change any success-path behavior.

## Acceptance Criteria

- With a bad API base, `node scripts/smoke-stripe.mjs --proxy --start-web` fails immediately with a clear hint about `--start-api`.
- `pnpm nx run @tejo/api:lint` remains green.

## Verification

- `cd tejospec && API_BASE_URL=http://localhost:59999/api/v1 node scripts/smoke-stripe.mjs --proxy --start-web`
- `cd tejospec && pnpm nx run @tejo/api:lint`
