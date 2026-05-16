---
id: continue-20260105-053000-run-auto-wrapper-e2e
timestamp: 2026-01-05T05:30:00Z
executed: true
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Validate the new `verify:live:with-stripe:proxy:auto:start` flow end-to-end (start services, run verify, run Stripe smoke, stop services).

## Scope

1. Run the end-to-end command:

- `cd tejospec && pnpm run verify:live:with-stripe:proxy:auto:start`

1. Confirm it:

- Starts `pnpm live` and waits for API health + web proxy health
- Runs `pnpm verify:live`
- Runs `pnpm smoke:stripe:proxy` (or `...:strict` if Stripe is configured)
- Stops the live processes on completion (success or failure)

## Constraints

- Do not modify code unless the run reveals a bug in the wrapper.

## Acceptance Criteria

- The command exits with a clear success/failure code.
- If it fails due to missing DB/seed/Stripe config, the output provides actionable next steps.
