---
id: continue-20260105-014500-wire-stripe-smoke-into-verify
timestamp: 2026-01-05T01:45:00Z
executed: false
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Make the Stripe smoke test easier to run consistently by adding dedicated package.json scripts for strict Stripe validation and an opt-in verify flow.

## Scope

1. Add `smoke:stripe:strict` that runs `scripts/smoke-stripe.mjs` with `--require-stripe`.

1. Add `verify:live:with-stripe` that runs the existing live verify steps plus the strict Stripe smoke.

1. Keep existing `verify:live` unchanged to avoid breaking environments that don’t have Stripe keys.

## Constraints

- No secrets committed.
- Keep changes minimal.

## Acceptance Criteria

- New scripts exist in `tejospec/package.json`.
- `pnpm nx run @tejo/api:lint` and `pnpm nx run @tejo/web:lint` still succeed.

## Verification

- `cd tejospec && pnpm smoke:stripe:strict -- --help` (should not error)
- `cd tejospec && pnpm nx run @tejo/api:lint`
- `cd tejospec && pnpm nx run @tejo/web:lint`
