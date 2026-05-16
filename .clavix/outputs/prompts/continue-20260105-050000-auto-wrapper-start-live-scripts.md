---
id: continue-20260105-050000-auto-wrapper-start-live-scripts
timestamp: 2026-01-05T05:00:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Add convenience `pnpm` scripts for the `verify:live:with-stripe:proxy:auto` wrapper’s new `--start-live` / `--force-live` options.

## Scope

1. Update [tejospec/package.json](tejospec/package.json):

- Add `verify:live:with-stripe:proxy:auto:start` (and alias) that runs the wrapper with `--start-live`.
- Add `verify:live:with-stripe:proxy:auto:start:force` (and alias) that runs the wrapper with `--start-live --force-live`.

## Constraints

- Do not change existing script behaviors; only add new scripts.

## Acceptance Criteria

- `cd tejospec && pnpm -s run verify:live:with-stripe:proxy:auto:start -- --print-plan` prints a plan including `pnpm live` and exits 0.
- `cd tejospec && pnpm nx run @tejo/api:lint` remains green.
