---
id: std-20251228-235956-continue-gha-wholesale-smoke
timestamp: 2025-12-28T23:59:56Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Add a GitHub Actions workflow that boots the minimal docker compose stack and runs the CI wholesale smoke test (`pnpm smoke:wholesale:ci`) in a repeatable way.

## Scope

- Create a new workflow under `.github/workflows/`.
- Workflow steps:
  - checkout
  - setup Node 20 + pnpm
  - start compose services needed for wholesale smoke (db, redis, backend)
  - wait for backend health (`/api/v1/health`)
  - run `pnpm smoke:wholesale:ci` in direct API mode
  - print compose logs on failure
  - bring stack down on completion

## Constraints

- No secrets committed to repo.
- Admin auth must come from GitHub Secrets (`SMOKE_ADMIN_TOKEN` OR `SMOKE_ADMIN_EMAIL` + `SMOKE_ADMIN_PASSWORD`).
- Keep runtime reasonable; do not run full frontend unless needed.

## Acceptance Criteria

- Workflow passes when secrets are provided and the stack is healthy.
- Workflow fails when admin secrets are missing (admin checks are required by the CI wrapper).
- Does not impact existing Nx workflow logic.
