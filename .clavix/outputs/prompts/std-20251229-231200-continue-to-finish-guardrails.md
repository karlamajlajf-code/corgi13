---
id: std-20251229-231200-continue-to-finish-guardrails
timestamp: 2025-12-29T23:12:00Z
executed: false
originalPrompt: "continue (finish to the end; keep canonical stable; avoid legacy drift)"
---

# Improved Prompt

## Objective

Continue hardening the TejoSpec canonical workspace (Nx + pnpm, `apps/api` + `apps/web`) so it is reliable, easy to run, and resistant to accidental drift from legacy folders. Preserve the passing wholesale verification path and the established health contract.

## Scope

- Non-destructive guardrails and documentation only (no deleting legacy work unless explicitly approved).
- Improve the canonical “preflight” experience by making `pnpm tejo:doctor` surface working-tree drift risk (legacy/generated changes) and point to the existing working-tree checker.
- Update key runbooks to include the working-tree guardrail and canonical-vs-legacy documentation links.

## Constraints

- Do not delete or modify legacy `backend/**` functionality beyond documentation/guardrails unless the user explicitly chooses a cleanup/migration path.
- Keep outputs concise and Windows-friendly.
- Prefer canonical commands and paths rooted at `tejospec/`.

## Acceptance Criteria

- `tejospec/scripts/doctor.mjs` detects legacy/generated working-tree changes (from `git status --porcelain=v1`) and:
  - Warns when legacy/generated changes exist.
  - Remains informational when only canonical folders changed.
  - Supports `--skip-working-tree` to bypass the check.
  - In `--strict` mode, warning behavior remains consistent (warnings cause non-zero exit).
- `tejospec/docs/MASTER_PLAN.md` mentions `pnpm check:working-tree` and `pnpm check:working-tree:strict`, and links to `docs/CANONICAL_WORKSPACE_AND_LEGACY.md`.
- `tejospec/docs/DOCKER_STACK.md` includes a short note about running the working-tree checker to avoid committing legacy/generated artifacts.
- Quick verification passes: `node tejospec/scripts/doctor.mjs --skip-endpoints --skip-db` runs successfully.

## Notes

- Canonical health endpoints must remain:
  - API: `http://localhost:8003/api/v1/health`
  - Web proxy: `http://localhost:3003/api/health`
- Wholesale proof path remains `pnpm verify:wholesale` (proxy-mode then direct API).
