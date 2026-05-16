---
id: std-20251229-000400-continue-verify-wholesale
timestamp: 2025-12-29T00:04:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue by adding a single cross-platform command to verify the wholesale catalog request funnel end-to-end in both proxy mode (web `/api/*`) and direct API mode.

## Context

- The stack is running at `http://localhost:3003` (web) and `http://localhost:8003/api/v1` (API).
- We already have `scripts/smoke-wholesale.mjs` and a doc runbook.

## Scope

- Add a new `scripts/verify-wholesale.mjs` that runs the smoke test twice:
  - proxy: sets `WEB_BASE_URL` (uses Next rewrite)
  - api: unsets `WEB_BASE_URL` and sets `API_BASE_URL`
- Add a `pnpm verify:wholesale` script for easy use.
- Update the smoke test doc to mention the new command.

## Acceptance Criteria

- `pnpm verify:wholesale` succeeds with exit code `0`.
- Output shows both proxy and api smoke runs passed.
