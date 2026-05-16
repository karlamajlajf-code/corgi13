---
id: continue-20260105-023000-smoke-stripe-proxy-scripts
timestamp: 2026-01-05T02:30:00Z
executed: true
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Add convenience `pnpm` scripts for running the Stripe smoke test through the Next.js proxy (with optional auto-start for API + web) while keeping defaults unchanged.

## Scope

1. Update [tejospec/package.json](tejospec/package.json):

- Add `smoke:stripe:proxy` (and alias) that runs `node ./scripts/smoke-stripe.mjs --proxy --start-api --start-web`.
- Add `smoke:stripe:proxy:strict` (and alias) that runs the same plus `--require-stripe`.
- Keep existing scripts and defaults unchanged.

## Constraints

- Do not change `verify:live` default behavior.
- Do not commit secrets.

## Acceptance Criteria

- `pnpm smoke:stripe:proxy -- --help` prints usage (no crash).
- `pnpm smoke:stripe:proxy:strict -- --help` prints usage (no crash).

## Verification

- `cd tejospec && pnpm smoke:stripe:proxy -- --help`
- `cd tejospec && pnpm smoke:stripe:proxy:strict -- --help`
