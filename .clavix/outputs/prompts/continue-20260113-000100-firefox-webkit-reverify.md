---
id: continue-20260113-000100-firefox-webkit-reverify
timestamp: 2026-01-13T00:01:00Z
executed: true
originalPrompt: "keep working"
---

# Improved Prompt

## Objective

Re-verify the Playwright E2E suite for the remaining desktop browser projects (**firefox** and **webkit**) after recent edits to `tests/e2e/specs/auth-flows.spec.ts`, ensuring the suite remains default-green and deterministic.

## Scope

- Preconditions:
  - Web storefront reachable at `http://localhost:3003`
  - API reachable at `http://localhost:8003` (health at `/api/v1/health`)
- Run Playwright E2E for:
  - `firefox`
  - `webkit`
- If failures occur:
  - First distinguish environment outage (connection refused / proxy 500 / health failing) from a real test regression.
  - Apply minimal fixes only if required.

## Constraints

- Prefer Nx invocations for tasks.
- Run deterministically with `STABLE_TEST_RUN=1` (1 worker).
- Avoid brittle selectors and locale-coupled assertions; prefer stable role/testid-based targeting and explicit wait-for-response patterns.
- Keep repo hygiene (no tracked runtime logs/artifacts).

## Implementation Steps

1. Smoke-check services:

   - `pnpm -s smoke:live`

2. Run Playwright for firefox:

   - `STABLE_TEST_RUN=1 pnpm -s nx run @tejo/root:e2e --skip-nx-cache -- --project=firefox`

3. Run Playwright for webkit:

   - `STABLE_TEST_RUN=1 pnpm -s nx run @tejo/root:e2e --skip-nx-cache -- --project=webkit`

4. Confirm working tree status:

   - `git status --porcelain`

## Acceptance Criteria

- Smoke check succeeds (expected endpoints return 200).
- `firefox` E2E succeeds.
- `webkit` E2E succeeds.
- No new unintended tracked files appear.

## Execution Notes

- Smoke check: OK (API health/docs 200; Web home/proxy health 200)
- E2E (firefox): **38 passed / 24 skipped**
- E2E (webkit): **38 passed / 24 skipped**
