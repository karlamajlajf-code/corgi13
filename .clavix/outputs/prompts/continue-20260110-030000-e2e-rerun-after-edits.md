---
id: continue-20260110-030000-e2e-rerun-after-edits
timestamp: 2026-01-10T03:00:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Re-verify and keep the default Playwright multi-project E2E run green after recent edits/formatting changes.

## Scope

- Re-read any recently modified E2E fixtures and critical UI test hooks to ensure they still match test expectations.
- Run the full Playwright suite via Nx (`@tejo/root:e2e`) with `--max-failures=1` against `http://localhost:3003`.
- If a failure occurs, fix it using stable locators (`data-testid` preferred), avoid locale-coupled assertions, and keep legacy/admin suites gated behind env flags.

## Constraints

- Use Nx (`nx run @tejo/root:e2e`) for test execution.
- Do not un-gate legacy/admin suites.
- Keep tests deterministic (stub network where needed).

## Acceptance Criteria

- `pnpm nx run @tejo/root:e2e -- --max-failures=1` completes without failures (across chromium/firefox/webkit/Mobile Chrome).

## Suggested Verification Commands

- `cd tejospec && pnpm nx run @tejo/root:e2e -- --max-failures=1`
