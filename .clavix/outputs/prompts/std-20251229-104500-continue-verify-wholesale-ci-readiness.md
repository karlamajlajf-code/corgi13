---
id: std-20251229-104500-continue-verify-wholesale-ci-readiness
timestamp: 2025-12-29T10:45:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue hardening Tejo Beauty wholesale readiness by validating that CI + Docker Compose + the “proxy health” path are consistent and actually work end-to-end.

## Scope

- Verify the current intended readiness contract:
  - API health: `http://localhost:8003/api/v1/health`
  - Web proxy health: `http://localhost:3003/api/health` (should proxy to API health via Next.js rewrites)
- Audit the repo for any remaining mismatches (workflow, docs, compose, scripts).
- Implement the smallest fixes required to make the readiness contract true.

## Constraints

- Keep changes minimal and targeted (no feature work).
- Prefer running verification via Nx where feasible.

## Acceptance Criteria

- `docker compose -f tejospec/docker-compose.yml config -q` succeeds.
- `frontend` healthcheck endpoint matches CI proxy readiness (`/api/health`).
- The repo contains a working path for `http://localhost:3003/api/health` to return 200 when the stack is up.
- Any docs that describe health/readiness match the implemented behavior.
