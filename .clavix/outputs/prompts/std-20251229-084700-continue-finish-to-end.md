---
id: std-20251229-084700-continue-finish-to-end
timestamp: 2025-12-29T08:47:00Z
executed: false
originalPrompt: "YES, continue, ... do it untill you made everything to the very end"
---

# Improved Prompt

## Objective

Finish the current TejoSpec “continue” thread end-to-end: reduce risk from the huge working tree by documenting canonical vs legacy paths, improve docs discoverability, and re-run the canonical verification checks to confirm wholesale readiness still passes.

## Scope

- Documentation + repo hygiene (no product behavior changes):
  - Add/refresh a canonical-vs-legacy guide under `tejospec/docs/`.
  - Update legacy quick reference docs to point to canonical runbooks and the wholesale verification docs.
- Verification:
  - Run `pnpm typecheck` in `tejospec/`.
  - Run `pnpm verify:wholesale` (proxy mode + direct API mode).
- Triage guidance:
  - Provide an actionable note on what is canonical (Nx apps) vs legacy folders (older implementations) and how to avoid committing accidental legacy changes.

## Constraints

- Do not delete or move large directory trees automatically.
- Keep the canonical runtime contract unchanged:
  - API health: `http://localhost:8003/api/v1/health`
  - Web proxy health: `http://localhost:3003/api/health`
- Prefer Nx/pnpm scripts over raw tooling.

## Acceptance Criteria

- New canonical-vs-legacy guide exists and is linked from relevant docs.
- Legacy quick reference clearly links to canonical: `tejospec/README.md` and `docs/MASTER_PLAN.md`.
- `pnpm typecheck` succeeds.
- `pnpm verify:wholesale` succeeds in both proxy + direct API modes.
