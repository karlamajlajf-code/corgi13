---
id: std-20251228-220300-tejo-verify-homepage-wholesale-cta
timestamp: 2025-12-28T22:03:00Z
executed: false
originalPrompt: "ok, continue according to your plan/schedule now"
---

# Improved Prompt

## Objective

Verify the new homepage **B2B / Wholesale** CTA is visible when running the stack via Docker and that it routes to the wholesale funnel pages.

## Scope

- Bring up/restart the compose stack services needed for the web UI:
  - `frontend` (port 3003)
  - `backend` (port 8003)
  - `db`, `redis` (dependencies)
- Confirm the homepage HTML contains the new `/wholesale` CTA.
- Confirm `GET /wholesale` and `GET /wholesale/catalog-request` return 200.

## Constraints

- Prefer existing repo scripts (`pnpm stack:*`) over ad-hoc docker commands.
- Do not change business logic.

## Acceptance Criteria

- `http://localhost:3003/` responds 200 and the response contains a link to `/wholesale`.
- `http://localhost:3003/wholesale` responds 200.
- `http://localhost:3003/wholesale/catalog-request` responds 200.
