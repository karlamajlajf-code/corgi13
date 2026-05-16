---
id: continue-20260111-150000-implement-account-testids
timestamp: 2026-01-11T15:00:00Z
executed: true
originalPrompt: "<conversation-summary> (continue work to remove remaining locale-coupled account assertions)"
---

# Improved Prompt

## Objective

Remove remaining locale-coupled (Croatian) selectors/assertions from the Playwright E2E suite by adding stable `data-testid` hooks on the Account **Notifications** and **Payments** pages, then updating `tejospec/tests/e2e/specs/account-pages.spec.ts` to use those testids.

## Scope

1. Add `data-testid` hooks to:
   - `tejospec/apps/web/src/pages/account/notifications.tsx`
   - `tejospec/apps/web/src/pages/account/payments.tsx`
2. Update `tejospec/tests/e2e/specs/account-pages.spec.ts`:
   - Replace Croatian text/role selectors in `Notifications Page` tests with `getByTestId`.
   - Replace Croatian text/role selectors in `Payments Page` tests with `getByTestId`.

## Constraints

- Do **not** change user-visible copy/translations.
- Prefer deterministic selectors:
  - Page-level: `*-page`, `*-title`
  - Actions: `*-save`, `*-saved`, `*-add`, `*-add-modal-*`
  - Row/toggle level: `notification-setting-<id>-email-toggle`, etc.
- Keep changes minimal and avoid introducing new test flakes.

## Acceptance Criteria

- `account-pages.spec.ts` contains **no locale-coupled assertions** for Notifications/Payments.
- Targeted E2E runs pass:
  - `pnpm nx run @tejo/root:e2e -- --project=chromium --grep="Notifications Page|Payments Page" --max-failures=1`
  - `pnpm nx run @tejo/root:e2e -- --project=webkit --grep="Notifications Page|Payments Page" --max-failures=1`

## Notes

- Notifications settings ids are stable (`orders`, `promotions`, `newsletter`, `rewards`, `reviews`, `stock`).
- Payments page has multiple "Add" buttons (header + empty state); give them distinct testids to avoid strict-mode duplicates.
