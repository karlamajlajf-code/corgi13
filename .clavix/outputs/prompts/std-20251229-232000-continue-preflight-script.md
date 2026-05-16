---
id: std-20251229-232000-continue-preflight-script
timestamp: 2025-12-29T23:20:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Add a single, canonical “preflight” entrypoint that enforces canonical-only changes (optional strict mode) and guides contributors away from legacy/generated drift. Keep the existing wholesale verification and health contract unchanged.

## Scope

- Add root scripts:
  - `pnpm tejo:preflight` (informational) → runs `pnpm check:working-tree` + `tejo doctor` with `--skip-endpoints --skip-db`
  - `pnpm tejo:preflight:strict` (enforcing) → runs `pnpm check:working-tree:strict` + `tejo doctor` in strict mode with `--skip-endpoints --skip-db`
- Update canonical docs to reference `tejo:preflight` as the recommended first command when returning to the repo.

## Constraints

- Do not delete or migrate legacy folders automatically.
- Do not change ports, health endpoints, or wholesale smoke/verify behavior.
- Keep scripts Windows-friendly.

## Acceptance Criteria

- New scripts exist in `tejospec/package.json` and run on Windows.
- `pnpm tejo:preflight` completes successfully on a typical machine (it may warn if services aren’t running).
- Docs mention the new preflight script in the canonical runbooks.
