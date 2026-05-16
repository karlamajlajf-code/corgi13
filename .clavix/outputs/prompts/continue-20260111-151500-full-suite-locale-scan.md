---
id: continue-20260111-151500-full-suite-locale-scan
timestamp: 2026-01-11T15:15:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Keep the default Playwright E2E suite **green and non-flaky** by (1) confirming no remaining locale-coupled selectors exist in the E2E specs and (2) running the full Nx Playwright suite to catch any regressions.

## Scope

1. Scan `tejospec/tests/e2e/specs/**` for Croatian/locale-coupled assertions and replace with stable selectors only if found.
2. Run the full E2E suite via Nx and fix the first failure/flaky selector if any occurs.

## Constraints

- Prefer `data-testid` selectors.
- Avoid localized text assertions (Croatian is the default locale).
- Keep default runs green; do not introduce new backend dependencies.

## Acceptance Criteria

- No obvious locale-coupled selectors remain in `tejospec/tests/e2e/specs/**`.
- `pnpm nx run @tejo/root:e2e -- --max-failures=1` passes.
