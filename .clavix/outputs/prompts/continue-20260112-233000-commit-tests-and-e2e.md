---
id: continue-20260112-233000-commit-tests-and-e2e
# Note: timestamp is UTC
timestamp: 2026-01-12T23:30:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue the commit-hygiene workflow by splitting the remaining working tree changes (16 modified files) into cohesive commits:

1. A small **web unit-test/Jest setup** commit.
2. A focused **Playwright/E2E stabilization** commit.

Then run the narrowest relevant verification to ensure the E2E suite still runs.

## Current State

- Branch: `001-build-comprehensive-ecommerce`
- `git status --porcelain=v1` shows 16 modified files, 0 staged.
- Remaining modified files:
  - `apps/web/jest.setup.ts`
  - `apps/web/src/__tests__/forgot-password.test.tsx`
  - `apps/web/src/__tests__/reset-password.test.tsx`
  - `playwright.config.ts`
  - `tests/e2e/fixtures/test-base.ts`
  - `tests/e2e/specs/*.spec.ts` (multiple)

## Scope

### 1) Commit: web unit tests

- Stage only:
  - `apps/web/jest.setup.ts`
  - `apps/web/src/__tests__/forgot-password.test.tsx`
  - `apps/web/src/__tests__/reset-password.test.tsx`
- Create commit message:
  - `test(web): update auth recovery tests`

### 2) Commit: Playwright config + E2E specs

- Stage only:
  - `playwright.config.ts`
  - `tests/e2e/fixtures/test-base.ts`
  - All modified files under `tests/e2e/specs/`
- Create commit message:
  - `test(e2e): stabilize playwright suite`

### 3) Verify

Run the narrowest relevant checks:

- `pnpm -s tejo:doctor:strict`
- `pnpm -s test:e2e -- --project=chromium --max-failures=1`

## Constraints / Guardrails

- Do not mix product-feature code changes into test commits.
- Keep each commit reviewable and scoped.
- Do not re-add generated artifacts (Playwright report output, etc.).

## Acceptance Criteria

- Two commits created (web unit tests, then E2E/playwright).
- `git status` is clean after commits.
- The verification commands above exit 0.
