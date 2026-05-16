---
id: continue-20260111-102200-remove-locale-coupled-account-assertions
timestamp: 2026-01-11T10:22:00Z
executed: false
originalPrompt: "CONTINUE"
---

# Improved Prompt

## Objective

Remove remaining locale-coupled assertions from account-related E2E tests (Croatian strings in selectors) while keeping default Playwright runs green.

## Scope

1. Add stable `data-testid` hooks to account pages:
   - `apps/web/src/pages/account/notifications.tsx`
   - `apps/web/src/pages/account/payments.tsx`
2. Update `tests/e2e/specs/account-pages.spec.ts` to use these test IDs instead of localized text / headings.

## Constraints

- Do not change user-visible copy or behavior.
- Prefer `data-testid` selectors.
- Keep changes minimal and low-risk.

## Acceptance Criteria

- The following specs pass in both Chromium and WebKit:
  - `pnpm nx run @tejo/root:e2e -- --project=chromium --grep="Notifications Page|Payments Page" --max-failures=1`
  - `pnpm nx run @tejo/root:e2e -- --project=webkit --grep="Notifications Page|Payments Page" --max-failures=1`
- `account-pages.spec.ts` no longer relies on Croatian strings for notifications/payments flows.
