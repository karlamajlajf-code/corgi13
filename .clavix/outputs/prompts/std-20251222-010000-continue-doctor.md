---
id: std-20251222-010000-continue-doctor
timestamp: 2025-12-22T03:00:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Add a single non-destructive command that diagnoses local dev prerequisites (DB/env/docker) and whether the live servers are reachable.

## Scope

- Add a Node script at `tejospec/scripts/doctor.mjs` that:

  - Checks Node version (>=20), and prints it
  - Checks Docker availability (`docker` CLI reachable and daemon responding)
  - Checks canonical API env file presence (`apps/api/.env`) without printing secrets
  - Optionally checks canonical DB compose status if Docker is available
  - Checks live endpoints when reachable:
    - `http://localhost:8003/api/v1/health`
    - `http://localhost:8003/api/docs`
    - `http://localhost:3003/`
  - Prints actionable next steps (e.g., `pnpm db:bootstrap:flow`, `pnpm live`, `pnpm verify:live`)
  - Supports `--strict` to exit non-zero when any required check fails

- Update `tejospec/package.json`:

  - Add `tejo:doctor` (avoid collision with pnpm built-in `doctor`)
  - Add Nx-friendly alias `tejo-doctor`

- Update `tejospec/docs/MASTER_PLAN.md`:
  - Document `pnpm tejo:doctor` under "Run Live (Local)" with its Nx equivalent
  - Add a completed checkbox entry
  - Add a Change Log entry

## Constraints

- pnpm-only
- No new dependencies
- Keep output safe (no secrets)
- Track changes only in `tejospec/docs/MASTER_PLAN.md`

## Acceptance Criteria

- `pnpm tejo:doctor` runs on Windows and gives useful guidance
- `pnpm exec nx run @tejo/root:tejo-doctor` works
