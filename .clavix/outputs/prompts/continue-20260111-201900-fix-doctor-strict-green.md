---
id: continue-20260111-201900-fix-doctor-strict-green
timestamp: 2026-01-11T20:19:00Z
executed: true
originalPrompt: "continue WORKING"
---

# Improved Prompt

## Objective

Make the repo developer sanity-check scripts default-green by ensuring:

- `pnpm smoke-live` exits 0 when web+api are up.
- `pnpm tejo:doctor:strict` exits 0 in the same “live servers reachable” scenario, even if Docker/DB containers are not being used.

## Scope

1. Reproduce the current failure output for `pnpm tejo:doctor:strict`.
2. Fix root causes of strict-mode warnings:
   - Stop tracking generated artifacts that trigger the doctor working-tree drift warning (e.g. Playwright report + last-run markers).
   - Adjust `scripts/doctor.mjs` so Docker/DB checks do not emit WARN (and thus fail strict mode) when all live endpoints are reachable.
3. Re-run:
   - `pnpm smoke-live`
   - `pnpm tejo:doctor:strict`

## Constraints

- Keep Playwright E2E behavior unchanged.
- Make minimal, safe changes.
- Preserve the usefulness of doctor output: if endpoints are down (or checks are explicitly skipped), Docker/DB warnings should still be actionable.

## Acceptance Criteria

- `pnpm smoke-live` exits 0.
- `pnpm tejo:doctor:strict` exits 0 (with live endpoints reachable).
- `pnpm check:working-tree` no longer reports generated output changes due to tracked artifacts.
