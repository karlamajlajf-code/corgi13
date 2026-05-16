---
id: std-20251222-022600-verify-tejo-doctor-db-health
timestamp: 2025-12-22T02:26:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Verify that the local diagnostics command `tejo:doctor` reports canonical DB container status/health via Docker Compose, and that strict mode behaves correctly.

## Scope

- Run `pnpm tejo:doctor` and confirm it prints per-service status lines for the canonical DB compose services (`db`, `redis`).
- Run `pnpm tejo:doctor:strict` and confirm it executes `node ./scripts/doctor.mjs --strict` and exits successfully when everything is healthy.
- If output is confusing or strict mode doesn’t resolve reliably, prefer documenting a reliable invocation (`pnpm run tejo:doctor:strict` or `pnpm tejo-doctor-strict`).

## Constraints

- pnpm-only.
- No new tracking documents; keep updates in tejospec/docs/MASTER_PLAN.md.
- No secrets in output.

## Acceptance Criteria

- Doctor output includes lines like:
  - `OK: DB service db: ... (healthy)`
  - `OK: DB service redis: ... (healthy)`
- Strict command runs the `--strict` variant and remains green in a healthy environment.
