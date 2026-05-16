---
id: continue-20260110-051500-e2e-stable-no-retries
timestamp: 2026-01-10T05:15:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Do a stricter stability verification of the default Playwright multi-project E2E suite by disabling retries and forcing a single worker run, so we catch any remaining flakes that were previously masked.

## Scope

- Run the default Nx-driven Playwright E2E suite (`@tejo/root:e2e`) across all projects with:
  - `STABLE_TEST_RUN=1` (forces 1 worker in `playwright.config.ts`)
  - `--retries=0`
  - `--max-failures=1` (stop at first failure)
- If a failure appears:
  - Apply the smallest stability fix (prefer `data-testid` selectors, resilient waits, and deterministic network stubs).
  - Re-run the narrowest possible subset first, then re-run the strict full suite again.

## Constraints

- Do not couple assertions to localized (Croatian) text.
- Keep admin/legacy suites gated behind env flags.
- Prefer Nx commands over running Playwright directly.

## Acceptance Criteria

- The strict run completes with no failures (or, if failures occur, they are fixed and the strict run becomes green).
