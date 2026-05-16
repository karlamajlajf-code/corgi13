---
id: continue-20260111-202800-clean-check-working-tree
timestamp: 2026-01-11T20:28:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Reduce noise from the working-tree drift tooling after untracking generated Playwright artifacts, so `pnpm check:working-tree` does not report staged deletions of generated outputs as “Generated output changes detected”.

## Scope

- Update `tejospec/scripts/check-working-tree.mjs` to ignore entries in generated roots (`playwright-report`, `test-results`, etc.) when the porcelain status indicates a deletion (staged or working-tree) under those generated roots.
- Keep legacy-change detection behavior unchanged.

## Constraints

- Do not change Playwright tests or runtime behavior.
- Do not hide real legacy folder changes.
- Keep output readable (still show a helpful summary for other changes).

## Acceptance Criteria

- Running `pnpm check:working-tree` no longer lists `playwright-report/index.html` and `test-results/.last-run.json` as generated output changes when they are staged deletions.
- `pnpm tejo:doctor:strict` remains green.
