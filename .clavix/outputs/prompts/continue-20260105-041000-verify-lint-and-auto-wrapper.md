---
id: continue-20260105-041000-verify-lint-and-auto-wrapper
timestamp: 2026-01-05T04:10:00Z
executed: true
originalPrompt: "(conversation handoff)"
---

# Improved Prompt

## Objective

Re-verify the workspace is still green after the new `verify:live:with-stripe:proxy:auto` wrapper, and confirm it behaves as expected in print-only mode.

## Scope

1. Run `@tejo/api:lint` through Nx from the `tejospec/` root to confirm the earlier anomalous terminal output was not a repo issue.

1. Run the wrapper in print mode to confirm it selects the correct Stripe smoke step without executing anything:

- `pnpm -s run verify:live:with-stripe:proxy:auto -- --print-plan`

1. (Optional) If `smoke:live` endpoints are already running locally, run the wrapper end-to-end:

- `pnpm run verify:live:with-stripe:proxy:auto`

## Constraints

- Do not modify source code.
- Do not print secrets.

## Acceptance Criteria

- `cd tejospec && pnpm nx run @tejo/api:lint` is green.
- `cd tejospec && pnpm -s run verify:live:with-stripe:proxy:auto -- --print-plan` exits `0` and prints the expected plan.
