---
id: std-20260103-151000-preflight-strict-legacy-cleanup
timestamp: 2026-01-03T15:10:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Get the canonical workspace (`tejospec/`) to pass `pnpm tejo:preflight:strict` end-to-end, while preserving any non-canonical work found under legacy folders.

## Context

- Canonical runtime + CI target is the pnpm + Nx monorepo under `tejospec/`.
- Strict preflight currently fails at `pnpm check:working-tree:strict` due to detected changes under legacy roots inside `tejospec/` (notably `tejospec/backend/**` and `tejospec/archive/**`).
- Canonical wholesale verification (`pnpm verify:wholesale` and `pnpm verify:wholesale:strict`) has been stabilized (routing prefix fix + default-off B2B gating + smoke checks).

## Scope

1. Identify all working-tree changes under legacy roots in `tejospec/`.
2. Isolate those legacy changes so strict mode passes (prefer safe reversible isolation via a scoped git stash that includes untracked files).
3. Re-run strict preflight from `tejospec/`.
4. Re-run `pnpm verify:wholesale:strict` as a regression check.

## Constraints

- Do not delete user work irreversibly.
- Do not modify legacy implementations unless explicitly required; instead, stash/restore them.
- Only fix issues introduced by canonical changes (apps/web, apps/api, scripts, docs).

## Acceptance Criteria

- `pnpm tejo:preflight:strict` passes when run from `tejospec/`.
- `pnpm verify:wholesale:strict` passes after cleanup.
- Any legacy diffs are preserved (stashed with a clear message), not lost.

## Implementation Notes

- Use `git status --porcelain=v1` (from repo root) to confirm staged/unstaged/untracked state.
- Prefer `git stash push -u -m "temp: isolate tejospec legacy changes for strict preflight" -- tejospec/backend tejospec/archive`.
- If strict preflight subsequently fails on other non-canonical roots (generated outputs), either clean those outputs or adjust local workflow, but do not relax strict checks.
