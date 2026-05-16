---
id: std-20251222-011200-continue-doctor-db-compose-status
timestamp: 2025-12-22T03:12:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Make `pnpm tejo:doctor` more actionable by reporting canonical DB Docker Compose container status/health (Postgres + Redis) in addition to env + endpoint reachability.

## Scope

- Update `tejospec/scripts/doctor.mjs`:

  - When Docker is available and `docker-compose.canonical-db.yml` exists, run:
    - `docker compose -f docker-compose.canonical-db.yml ps --format json`
  - Parse compose `ps` output (NDJSON) and report each service with state/health.
  - If containers aren’t running or health is bad, print a warning and suggest `pnpm db:up` / `pnpm db:bootstrap:flow`.

- Update `tejospec/docs/MASTER_PLAN.md`:
  - Note that `tejo:doctor` reports canonical DB container status.
  - Add a completed checkbox entry and changelog line.

## Constraints

- pnpm-only
- No new dependencies
- Safe output (no secrets)

## Acceptance Criteria

- `pnpm tejo:doctor` prints a line per DB service (state/health) when Docker is available.
- `pnpm tejo:doctor:strict` exits non-zero when DB services are down/unhealthy.
