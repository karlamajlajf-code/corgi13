---
id: continue-20260105-043000-auto-wrapper-start-live
timestamp: 2026-01-05T04:30:00Z
executed: true
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Improve the opt-in `verify:live:with-stripe:proxy:auto` wrapper so it can optionally start the live dev servers before running verification.

## Scope

1. Update [tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs](tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs):

- Add a `--start-live` option:
  - When set, start `pnpm live` (optionally with `--force-live` → `pnpm live --force`) before running any verification.
  - Wait for API health (`http://localhost:8003/api/v1/health`) and web proxy health (`http://localhost:3003/api/health`) to become reachable (bounded by a timeout).
  - Ensure the started process is cleaned up on exit (Windows-safe process tree termination).
- Extend `--help` and `--print-plan` output to include the live-start step when applicable.
- Preserve existing behavior and safety defaults when `--start-live` is not provided.

1. Preserve existing behavior and safety defaults when `--start-live` is not provided.

## Constraints

- Do not change default verification scripts in `package.json`.
- Do not print secrets.

## Acceptance Criteria

- `cd tejospec && node scripts/verify-live-with-stripe-proxy-auto.mjs --help` exits 0.
- `cd tejospec && node scripts/verify-live-with-stripe-proxy-auto.mjs --print-plan --start-live` prints a 3-step plan and exits 0 without starting anything.
- `cd tejospec && pnpm nx run @tejo/api:lint` remains green.
