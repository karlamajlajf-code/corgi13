---
id: continue-20260105-063000-stripe-route-fast-fail
timestamp: 2026-01-05T06:30:00Z
executed: true
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Make Stripe verification failures more actionable and faster when the running API does not expose Stripe routes.

## Scope

1. Update [tejospec/scripts/smoke-stripe.mjs](tejospec/scripts/smoke-stripe.mjs):

- When the Stripe endpoints return 404, print a clear hint that the API on port 8003 is likely not the tejospec Nest API (or needs a restart), and that `--start-api` won’t replace an already-running process.

1. Update [tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs](tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs):

- When running with `--start-live`, probe `GET ${API_BASE_URL}/stripe/payment-methods` (unauthenticated) after health checks.
- If that probe returns 404, fail fast before running `verify:live`, so we don’t spend time on DB-backed flow tests only to fail later.

## Constraints

- Keep safe defaults; do not kill processes automatically.
- Do not print secrets.

## Acceptance Criteria

- Running `node scripts/smoke-stripe.mjs --proxy --start-api --start-web` on an API missing Stripe routes prints an actionable hint.
- Running `node scripts/verify-live-with-stripe-proxy-auto.mjs --start-live` fails fast with a Stripe-route-missing message (before `verify:live`).
- `pnpm nx run @tejo/api:lint` remains green.
