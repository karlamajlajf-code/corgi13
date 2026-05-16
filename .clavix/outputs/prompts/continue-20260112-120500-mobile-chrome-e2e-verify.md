---
id: continue-20260112-120500-mobile-chrome-e2e-verify
timestamp: 2026-01-12T12:05:00Z
executed: true
originalPrompt: "Continue the E2E verification work: run the remaining Playwright project(s), starting with Mobile Chrome, keep the suite default-green and the repo clean."
---

# Improved Prompt

## Objective

Verify and (if needed) stabilize the Playwright E2E suite for the **Mobile Chrome** project so the repo remains **default-green** across the browser matrix.

## Scope

- Preconditions:
  - Next.js storefront is reachable at `http://localhost:3003`
  - NestJS API is reachable at `http://localhost:8003` (`/api/v1/health`)
- Run only the Playwright project:
  - `Mobile Chrome`
- If failures occur:
  - First determine whether it’s an **environment outage** (servers down / proxy 500) vs a **test flake/regression**.
  - Apply the **smallest** stability fix (prefer existing patterns already used in this repo: stable selectors, explicit waits for URL/state, waiting on stubbed responses, avoiding strict-mode collisions).

## Constraints

- Prefer **Nx** for running tasks.
- Keep tests deterministic with `STABLE_TEST_RUN=1`.
- Do not introduce locale-coupled assertions or brittle selectors.
- Keep repo hygiene:
  - Do not commit Playwright artifacts.
  - Do not accidentally track `logs/` or other local runtime output.

## Implementation Steps

1. **Smoke-check** live services:
   - `pnpm -s smoke:live`
2. Run Mobile Chrome E2E (no cache):
   - `STABLE_TEST_RUN=1 pnpm -s nx run @tejo/root:e2e --skip-nx-cache -- --project="Mobile Chrome"`
3. If failures occur:
   - If connect/refused/500: restart services and re-run smoke check before re-running E2E.
   - If test flake: patch only the affected spec(s), then re-run the minimal failing subset first, then re-run the full Mobile Chrome project.
4. Confirm `git status --porcelain` is clean except for intended changes.

## Acceptance Criteria

- `pnpm -s smoke:live` succeeds (all expected 200s).
- Mobile Chrome E2E run succeeds (no failures).
- Any changes are minimal, justified by the failure mode, and do not reduce determinism.
- Working tree remains free of newly tracked runtime artifacts.

## Execution Notes

- Smoke check: OK (API health/docs 200; Web home/proxy health 200)
- E2E: `Mobile Chrome` project: **38 passed / 24 skipped**
