---
id: std-20251229-121500-continue-align-and-verify-health-contract
timestamp: 2025-12-29T12:15:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Ensure wholesale readiness health checks are consistent and proven end-to-end across Docker Compose, scripts, docs, and CI.

## Scope

- Tejospec workspace only.
- Align and verify the health contract for:
  - Direct API health: `http://localhost:8003/api/v1/health`
  - Web proxy health: `http://localhost:3003/api/health` (Next rewrites `/api/*` → backend `/api/v1/*`)

## Constraints

- Minimal, surgical edits.
- Do not change public behavior beyond fixing broken health-probing.
- Prefer updating existing scripts/docs rather than adding new ones.

## Tasks

1. Repo-wide audit for `/health` / `/api/health` references (scripts/docs/workflows/compose).
2. Fix any mismatches so:
   - Proxy/web mode probes `WEB_BASE_URL + '/api/health'` (not `/health`).
   - API mode probes `API_BASE_URL + '/health'` where `API_BASE_URL` ends in `/api/v1`.
3. Verify runtime behavior:
   - `docker compose up -d db redis backend frontend`
   - `curl -fsS http://localhost:8003/api/v1/health`
   - `curl -fsS http://localhost:3003/api/health`
4. Run the narrowest wholesale verification command that exercises both modes (or the existing combined verify script).

## Acceptance Criteria

- No remaining inconsistencies where scripts probe `/health` on the web host instead of `/api/health`.
- Local compose stack becomes healthy and both health endpoints return success.
- Wholesale smoke/verify completes (or fails only for unrelated pre-existing issues, which are reported clearly).
