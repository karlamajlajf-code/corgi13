---
id: continue-20260105-024500-smoke-stripe-print-config
timestamp: 2026-01-05T02:45:00Z
executed: true
originalPrompt: 'go, stop telling me what "you will do", do it instead!'
---

# Improved Prompt

## Objective

Add a `--print-config` option to the Stripe smoke script to print the resolved URLs/flags/timeouts and exit cleanly, to make proxy/env debugging fast.

## Scope

1. Update [tejospec/scripts/smoke-stripe.mjs](tejospec/scripts/smoke-stripe.mjs):

- Add `--print-config` flag to `usage()`.
- If `--print-config` is present, print:
  - `API_BASE_URL`, `WEB_BASE_URL` (raw), `effectiveWebBaseUrl`
  - `HTTP_BASE_URL`, `BASE_URL`
  - `API_HEALTH_URL`, `PROXY_HEALTH_URL`, `HEALTH_URL`
  - `TIMEOUT_MS`, `STARTUP_TIMEOUT_MS`
  - boolean flags: `useProxy`, `shouldStartApi`, `shouldStartWeb`, `requireStripe`
- Exit `0` without starting any processes or calling health endpoints.

## Acceptance Criteria

- `cd tejospec && node scripts/smoke-stripe.mjs --print-config` exits `0`.
- `cd tejospec && node scripts/smoke-stripe.mjs --proxy --print-config` exits `0`.

## Verification

- `cd tejospec && node scripts/smoke-stripe.mjs --print-config`
- `cd tejospec && node scripts/smoke-stripe.mjs --proxy --print-config`
