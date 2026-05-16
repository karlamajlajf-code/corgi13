---
id: continue-20260105-070000-auto-wrapper-takeover-ports
timestamp: 2026-01-05T07:00:00Z
executed: true
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Make the opt-in Stripe auto-verify flow more reliable when ports are already occupied by the wrong process by adding an explicit, destructive `--takeover-ports` option.

## Scope

1. Update [tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs](tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs):

- Add `--takeover-ports` (only valid with `--start-live`).
- When enabled, run `npx --yes kill-port 3003 8003` before starting `pnpm live`.
- Include this step in `--print-plan` output.
- Keep the default behavior unchanged when the flag is not provided.

1. Update [tejospec/package.json](tejospec/package.json):

- Add convenience script(s) for the takeover mode.

## Constraints

- The takeover behavior must be strictly opt-in.
- Do not print secrets.

## Acceptance Criteria

- `cd tejospec && node scripts/verify-live-with-stripe-proxy-auto.mjs --print-plan --start-live --takeover-ports` prints a plan including `kill-port` and exits 0.
- `cd tejospec && pnpm -s run verify:live:with-stripe:proxy:auto:start:takeover -- --print-plan` prints the same plan.
- `cd tejospec && pnpm nx run @tejo/api:lint` remains green.
