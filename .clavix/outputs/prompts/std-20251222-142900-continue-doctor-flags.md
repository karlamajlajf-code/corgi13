---
id: std-20251222-142900-continue-doctor-flags
timestamp: 2025-12-22T14:29:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Make `pnpm tejo:doctor` more usable in "preflight" scenarios by adding opt-in flags to skip checks that are expected to fail when services are not running yet (while keeping the current default behavior intact).

## Scope

- Update `tejospec/scripts/doctor.mjs`:

  - Add `--skip-endpoints` (alias: `--no-endpoints`) to skip live HTTP endpoint probes.
  - Add `--skip-db` (alias: `--no-db`) to skip canonical DB compose container status/health checks (and avoid warning about the compose file if DB checks are skipped).
  - When a section is skipped, print a single `OK:` line indicating it was skipped and which flag did it.

- Update `tejospec/docs/MASTER_PLAN.md`:
  - Add a completed checkbox item for the new doctor flags.
  - Add a changelog entry noting the new flags.

## Constraints

- pnpm-only
- No new dependencies
- No secrets in output
- Keep existing doctor defaults and strict-mode semantics

## Acceptance Criteria

- `pnpm tejo:doctor --skip-endpoints` does not emit endpoint reachability warnings.
- `pnpm tejo:doctor --skip-db` does not run `docker compose ... ps` checks and does not warn about `docker-compose.canonical-db.yml`.
- Running `pnpm tejo:doctor` without flags behaves the same as before.
- `pnpm tejo:doctor:strict` behavior remains unchanged unless skip flags are provided.
