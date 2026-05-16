---
id: continue-20260106-000100-reverify-auto-wrapper-after-edits
timestamp: 2026-01-06T00:01:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue hardening the Stripe verification wrapper after recent external edits, ensuring behavior remains safe-by-default and the opt-in takeover path reliably replaces wrong/stale listeners on ports 8003/3003.

## Scope

1. Re-read and validate behavior in [tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs](tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs) after the recent edits.
1. Confirm the API Stripe routes are expected to exist by checking [tejospec/apps/api/src/modules/stripe](tejospec/apps/api/src/modules/stripe) wiring.
1. Verify wrapper behavior:
   - `--print-plan` output is stable for `--start-live`, `--force-live`, and `--takeover-ports`.
   - `--takeover-ports` remains strictly opt-in and still requires `--start-live`.
1. Run an end-to-end takeover attempt to validate the environment is corrected:
   - `pnpm run verify:live:with-stripe:proxy:auto:start:takeover`
1. Keep Nx green with a targeted lint run.

## Constraints

- Defaults remain non-destructive; destructive actions must be strictly opt-in.
- Do not print secrets.
- Keep Windows compatibility.

## Acceptance Criteria

- `node scripts/verify-live-with-stripe-proxy-auto.mjs --print-plan --start-live --takeover-ports` prints a plan including `kill-port` and exits 0.
- `pnpm run verify:live:with-stripe:proxy:auto:start:takeover` successfully replaces any wrong listener on ports 8003/3003 and proceeds to `verify:live` + Stripe smoke.
- `pnpm nx run @tejo/api:lint` remains green.
