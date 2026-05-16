---
id: continue-20260110-020000-e2e-webkit-profile-authme-stubs
timestamp: 2026-01-10T02:00:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Get the default Playwright multi-project E2E suite back to green (Chromium, Firefox, WebKit, Mobile Chrome) against the Next.js storefront at `http://localhost:3003`, focusing on the remaining WebKit failure in `account-pages.spec.ts` (logout/redirect during profile save).

## Scope

1. **Stabilize auth during account/profile tests** by stubbing the missing auth hydration endpoints:

   - `GET **/api/auth/me`
   - `PATCH **/api/auth/me`

   in `tejospec/tests/e2e/specs/account-pages.spec.ts` (preferably inside the `loginAsAdmin()` helper so it applies to all account page tests).

2. **Remove locale-coupled selectors/assertions** from the profile editing tests by introducing stable `data-testid` hooks in the UI and switching the tests to use them:

   - Add `data-testid` attributes to profile page controls and messages in `tejospec/apps/web/src/pages/account/profile.tsx`:
     - `profile-edit`, `profile-save`, `profile-cancel`, `profile-saved`
   - Update `account-pages.spec.ts` to click/assert using these test ids instead of Croatian button text and translated success text.

3. **Eliminate EntranceLoader click interception flakiness globally** by setting the sessionStorage flag that disables the one-time overlay for all E2E tests:

   - In `tejospec/tests/e2e/fixtures/test-base.ts`, add a `page.addInitScript` that sets `sessionStorage['tejo:entrance:shown:v1']='1'` (wrapped in try/catch).

## Constraints

- Keep the E2E suite targeting `http://localhost:3003`.
- Prefer `data-testid` selectors over text/CSS selectors.
- Avoid adding new dependencies.
- Keep legacy/admin suites gated behind env flags (do not un-gate).

## Acceptance Criteria

- `tejospec/tests/e2e/specs/account-pages.spec.ts` passes on `--project=webkit`.
- Full Playwright run (all configured projects) passes with `--max-failures=1`.
- Profile tests no longer assert Croatian-specific success strings or click Croatian-labeled buttons.

## Suggested Verification Commands

- From `tejospec/`:
  - Run WebKit account pages first:
    - `nx e2e <e2e-project> -- --project=webkit tests/e2e/specs/account-pages.spec.ts --workers=1`
  - Then run full suite:
    - `nx e2e <e2e-project> -- --max-failures=1`
      (If the exact Nx e2e project name is unclear, list it with `nx show projects` and select the Playwright project.)
