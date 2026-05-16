---
id: continue-20260110-003100-e2e-keep-default-multiproject-green
timestamp: 2026-01-10T00:31:00Z
executed: false
originalPrompt: "continue"
context: "Default Playwright multi-project run should remain green against apps/web on http://localhost:3003 after recent edits to user-authentication.spec.ts."
---

# Improved Prompt

## Objective

Keep the default Playwright E2E suite **green across all configured projects** (Chromium/Firefox/WebKit + Mobile) for the `apps/web` storefront served at `http://localhost:3003`.

## Scope

- Re-run the default Playwright suite with `--max-failures=1` to surface the next failure (if any).
- If a failure is found, fix it using stable, layout-agnostic selectors (prefer `data-testid`) and by avoiding locale-coupled assertions.
- Ensure recent edits in `tejospec/tests/e2e/specs/user-authentication.spec.ts` do not introduce flakes or cross-project regressions.

## Constraints

- Keep legacy/admin suites opt-in (do not re-enable them by default).
- Avoid assertions based on translated UI strings where possible.
- Prefer minimal, targeted changes that address the specific failure.

## Acceptance Criteria

- Full default run passes with no failures:
  - `STABLE_TEST_RUN=1 pnpm exec playwright test --workers=1 --max-failures=1 --reporter=line`
- Any fixes include focused verification on the failing spec/project.
