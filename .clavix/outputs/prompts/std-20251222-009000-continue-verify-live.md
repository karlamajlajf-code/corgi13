---
id: std-20251222-009000-continue-verify-live
timestamp: 2025-12-22T02:50:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Add a one-command local verification that confirms the live servers are responding and that the DB-backed happy-path flow still works.

## Scope

- Update `tejospec/package.json` scripts:
  - Add `verify:live`: runs `pnpm smoke:live` then `pnpm smoke:flow`
  - Add Nx-friendly alias `verify-live`
- Update `tejospec/docs/MASTER_PLAN.md`:
  - Document `pnpm verify:live` under "Run Live (Local)" with its Nx equivalent
  - Add a completed checkbox entry
  - Add a Change Log line

## Constraints

- pnpm-only
- Keep changes minimal; no new dependencies
- Do not add new tracking docs; only update `tejospec/docs/MASTER_PLAN.md`

## Acceptance Criteria

- `pnpm verify:live` runs both smokes in order and exits non-zero on failure
- `pnpm exec nx run @tejo/root:verify-live` works
