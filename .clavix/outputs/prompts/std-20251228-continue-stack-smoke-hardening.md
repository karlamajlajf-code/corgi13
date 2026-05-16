---
id: std-20251228-continue-stack-smoke-hardening
timestamp: 2025-12-28T12:00:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue hardening the Tejospec wholesale operational pipeline by ensuring Docker stack healthchecks align with the API routing, and making the wholesale smoke test resilient and easy to run.

## Scope

- Fix any Docker Compose healthchecks that point at incorrect API paths (Nest global prefix is `api/v1`).
- Re-validate and (if needed) harden `scripts/smoke-wholesale.mjs` so DB verification can be skipped gracefully when Docker isn’t available.
- Keep changes small, targeted, and compatible with Nx + pnpm workflows.

## Constraints

- Prefer Nx for checks (`pnpm nx ...`).
- Do not introduce unrelated refactors.
- If runtime services aren’t available, provide a verification path that still exercises the script (e.g., help mode, or `SMOKE_VERIFY_DB=0`).

## Acceptance Criteria

- `docker-compose.yml` backend healthcheck hits the correct URL (`/api/v1/health`).
- Smoke script does not hard-exit just because Docker Compose is unavailable (it should clearly `SKIP` DB verification unless the user explicitly wants strict mode).
- Focused verification passes (typecheck + lint for affected areas, and smoke script help output).
