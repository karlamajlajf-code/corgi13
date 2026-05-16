---
id: continue-20260111-153000-verify-testids-after-format
timestamp: 2026-01-11T15:30:00Z
executed: true
originalPrompt: "contInue"
---

# Improved Prompt

## Objective

After recent formatting/edits, confirm the Account Notifications/Payments `data-testid` hooks still exist and the Playwright E2E specs remain locale-agnostic and green.

## Scope

1. Verify `data-testid` hooks remain in:
   - `tejospec/apps/web/src/pages/account/notifications.tsx`
   - `tejospec/apps/web/src/pages/account/payments.tsx`
2. Re-run focused Playwright E2E for Notifications/Payments in Chromium and WebKit.

## Constraints

- Do not change translations/copy.
- Keep selectors deterministic via `getByTestId`.

## Acceptance Criteria

- Focused E2E runs pass:
  - `pnpm nx run @tejo/root:e2e -- --project=chromium --grep="Notifications Page|Payments Page" --max-failures=1`
  - `pnpm nx run @tejo/root:e2e -- --project=webkit --grep="Notifications Page|Payments Page" --max-failures=1`
