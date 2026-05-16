---
id: std-20251222-030000-continue-doctor-pnpm-compose-checks
timestamp: 2025-12-22T03:00:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Improve `pnpm tejo:doctor` by checking two additional prerequisites that commonly break local dev, especially on Windows:

- `pnpm` is installed and meets the repo’s required major version.
- Docker Compose v2 plugin (`docker compose ...`) is available before running compose commands.

## Scope

- Update tejospec/scripts/doctor.mjs:

  - Add a `pnpm --version` check.
    - Read repo expectation from `package.json` (`packageManager: pnpm@...` or `engines.pnpm`).
    - Warn if pnpm is missing or major version is below required.
  - Add a `docker compose version` check.
    - If missing, warn clearly that Docker Compose v2 is required.
    - Skip compose `ps` checks if compose is unavailable (avoid misleading DB-down warnings).

- Update tejospec/docs/MASTER_PLAN.md:
  - Add a completed task item for the new doctor checks.
  - Add a changelog line.

## Constraints

- pnpm-only
- No new dependencies
- No secrets in output

## Acceptance Criteria

- `pnpm tejo:doctor` prints an OK/WARN line about pnpm and docker compose availability.
- Existing doctor checks remain unchanged and strict-mode semantics are preserved.
- `pnpm exec nx run @tejo/root:tejo-doctor` continues to work.
