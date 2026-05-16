---
id: continue-20260105-031500-verify-live-with-stripe-proxy-auto
timestamp: 2026-01-05T03:15:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Add an opt-in `verify:live:with-stripe:proxy:auto` command that runs strict proxy Stripe smoke only when Stripe looks configured (based on `apps/api/.env`), otherwise runs the non-strict proxy smoke.

## Scope

1. Add a new script [tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs](tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs):

- Detect Stripe configuration by checking for a non-empty `STRIPE_SECRET_KEY=` line in `apps/api/.env` (do not print the value).
- Provide `--help` and `--print-plan` options that do not run any commands.
- Run `pnpm verify:live` first, then:
  - If configured: `pnpm smoke:stripe:proxy:strict`
  - Else: `pnpm smoke:stripe:proxy`

1. Update [tejospec/package.json](tejospec/package.json):

- Add `verify:live:with-stripe:proxy:auto` (and alias) to execute the new script.

## Constraints

- Do not change default verification scripts.
- Do not print secrets.

## Acceptance Criteria

- `cd tejospec && node scripts/verify-live-with-stripe-proxy-auto.mjs --print-plan` exits `0`.
- `cd tejospec && pnpm nx run @tejo/api:lint` remains green.
