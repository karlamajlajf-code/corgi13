---
id: continue-20260105-030000-verify-live-with-stripe-proxy
timestamp: 2026-01-05T03:00:00Z
executed: true
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Add an opt-in verification script that runs the existing live verification plus a strict Stripe smoke check through the Next.js proxy.

## Scope

1. Update [tejospec/package.json](tejospec/package.json):

- Add `verify:live:with-stripe:proxy` (and alias) to run `pnpm verify:live && pnpm smoke:stripe:proxy:strict`.
- Keep `verify:live` and `verify:live:with-stripe` unchanged.

## Acceptance Criteria

- `cd tejospec && pnpm -s run verify:live:with-stripe:proxy --help` exits without crashing (pnpm help output).
- `pnpm nx run @tejo/api:lint` remains green.
