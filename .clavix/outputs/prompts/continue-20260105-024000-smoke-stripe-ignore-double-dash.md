---
id: continue-20260105-024000-smoke-stripe-ignore-double-dash
timestamp: 2026-01-05T02:40:00Z
executed: true
originalPrompt: "CONTINUE WORKING!"
---

# Improved Prompt

## Objective

Make the Stripe smoke script tolerant of pnpm argument forwarding by ignoring a standalone `--` in `process.argv`, without changing behavior for any real flags.

## Scope

1. Update [tejospec/scripts/smoke-stripe.mjs](tejospec/scripts/smoke-stripe.mjs):

- Filter `process.argv.slice(2)` to drop a literal `--` token.
- Keep all existing flags and behavior unchanged.

## Acceptance Criteria

- `pnpm smoke:stripe:proxy -- --help` prints usage (no crash).
- `pnpm smoke:stripe:proxy:strict -- --help` prints usage (no crash).

## Verification

- `cd tejospec && pnpm smoke:stripe:proxy -- --help`
- `cd tejospec && pnpm smoke:stripe:proxy:strict -- --help`
