---
id: std-20251222-024000-continue-doctor-compose-robust
timestamp: 2025-12-22T02:40:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Make `pnpm tejo:doctor` more robust across Docker Compose versions and failure modes:

- Correctly parse both NDJSON and JSON-array outputs from `docker compose ps --format json`.
- Distinguish “compose json format unsupported” from “containers not running”.
- Ensure endpoint checks never hang and always clean up timers.

## Scope

- Update tejospec/scripts/doctor.mjs:

  - Enhance compose parsing to accept:
    - NDJSON (one JSON object per line)
    - A JSON array of objects
    - A single JSON object
  - If `docker compose ... ps --format json` fails (command error/null stdout), fall back to `docker compose ... ps` and emit a clear warning that JSON formatting is unsupported.
  - If JSON parsing succeeds but returns zero services, warn that canonical DB containers do not appear to be running and suggest `pnpm db:start` / `pnpm db:up`.
  - Refactor `checkHttp()` to always clear the AbortController timeout in both success and failure cases.

- Update tejospec/docs/MASTER_PLAN.md:
  - Add a concise changelog line noting doctor robustness improvements.

## Constraints

- pnpm-only
- No new dependencies
- No secrets in output

## Acceptance Criteria

- With the current Docker Compose version, doctor output remains stable (still reports DB service status/health).
- Doctor no longer risks hanging on endpoint checks.
- On environments where `docker compose ps --format json` is unsupported, doctor prints a warning about the limitation instead of incorrectly reporting the DB as down.
