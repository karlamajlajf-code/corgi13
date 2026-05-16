---
id: std-20260103-161600-continue-lint-and-reverify
timestamp: 2026-01-03T16:16:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue to a fully verified end-state by running focused Nx lints for the canonical apps and fixing any failures introduced by recent changes.

## Scope

- Lint the canonical apps (prefer Nx):
  - `pnpm exec nx run @tejo/api:lint`
  - `pnpm exec nx run @tejo/web:lint`
- If lint succeeds, re-run strict canonical guardrail:
  - `pnpm tejo:preflight:strict`
- (Optional, if time) Re-run the core wholesale proof point:
  - `pnpm verify:wholesale:strict`

## Constraints

- Do not add new product features.
- Do not modify legacy folders (`backend/`, `frontend/`, `archive/`).
- Prefer Nx tasks (`nx run ...`) over underlying tooling.

## Acceptance Criteria

- Nx lints for `@tejo/api` and `@tejo/web` succeed.
- `pnpm tejo:preflight:strict` succeeds.
- No new failures in existing smoke/verify scripts.
