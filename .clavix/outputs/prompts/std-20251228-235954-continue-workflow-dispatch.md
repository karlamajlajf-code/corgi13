---
id: std-20251228-235954-continue-workflow-dispatch
timestamp: 2025-12-28T23:59:54Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Make the wholesale smoke CI workflow easier to run and debug by adding a manual trigger (`workflow_dispatch`) and a simple mode switch.

## Scope

- Update `.github/workflows/smoke-wholesale.yml` to add `workflow_dispatch`.
- Support an optional `mode` input:
  - `api` (default): start `db`, `redis`, `backend`; run smoke in direct API mode.
  - `proxy`: also start `frontend`; run smoke via `WEB_BASE_URL` so it exercises the Next `/api/*` proxy.

## Constraints

- Keep existing branch triggers and `paths` filters.
- No credentials committed; admin auth still comes from GitHub Secrets.

## Acceptance Criteria

- You can manually run the workflow from GitHub UI.
- Default behavior for push/PR remains direct API mode.
- YAML stays valid.
