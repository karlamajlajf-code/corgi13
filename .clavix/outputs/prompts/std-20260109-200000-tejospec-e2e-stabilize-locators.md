---
id: std-20260109-200000-tejospec-e2e-stabilize-locators
project: tejospec
timestamp: 2026-01-09T20:00:00Z
executed: true
executedAt: 2026-01-09T20:10:00Z
originalPrompt: "continue — fix remaining Playwright E2E failures (strict-mode locator ambiguity, brittle assertions) and keep i18n Croatian-first with no German-coupled strings"
---

# Improved Prompt

## Objective

Stabilize the remaining failing Playwright E2E tests in `tejospec/` by removing strict-mode locator ambiguity and brittle assertions, while keeping the core constraints:

- Croatian is the primary UX language.
- Tests must not depend on German text.
- Prefer stable selectors (role + level, ids, hrefs) over loose text regex.

## Scope

Update Playwright specs (no app UI copy changes unless strictly necessary):

- `tejospec/tests/e2e/specs/account-pages.spec.ts`
- `tejospec/tests/e2e/specs/auth-flows.spec.ts`

Focus on failures that are currently caused by:

1. Strict-mode violations from text/regex matching multiple elements.
2. Toggle tests asserting class changes using a non-specific selector.
3. Forgot-password success test failing because the API endpoint is not reliably available in the E2E environment.
4. Password requirement test assuming zero requirements are met for a weak password (but lowercase is typically satisfied).

## Constraints / Requirements

- Do not introduce German literals into tests.
- Croatian assertions are acceptable when a text assertion is unavoidable.
- Avoid fragile `getByText(/.../)` across the whole page when the same string can appear in multiple places.
- Playwright should run against the externally running web app at `http://localhost:3003` (baseURL already aligned in config).

## Acceptance Criteria

- `account-pages.spec.ts` no longer fails due to strict-mode ambiguity:

  - Notifications page heading assertion targets the main page title only.
  - Payments page heading assertion targets the main page title only.
  - Add-payment modal assertions are scoped to the modal container.
  - Available payment method assertions are scoped to the “Available payment methods” section to avoid duplicate matches (e.g., VISA).
  - Notification toggle test uses a deterministic row-scoped toggle and verifies state change reliably.

- `auth-flows.spec.ts`:
  - Forgot-password success test stubs `POST **/api/auth/forgot-password` to return success and asserts the success UI state.
  - Password requirement test asserts that a weak password satisfies fewer than all requirements (not necessarily zero), and that a strong password satisfies all.

## Implementation Notes

- Use `getByRole('heading', { level: 1, ... })` to disambiguate page titles.
- When asserting modal content, scope locators to the modal container (`div.fixed.inset-0...`) rather than the whole page.
- For the notifications toggle test, clear `notification-settings` from `localStorage` before navigating, then locate the row containing "Status narudzbe" and toggle its email switch.
- For forgot-password success, use `page.route('**/api/auth/forgot-password', ...)` and fulfill a 200 JSON response.

## Verification

Run a focused Playwright run against the two specs (Chromium, 1 worker) and ensure failures reduce/resolve:

- `pnpm -C tejospec exec playwright test tests/e2e/specs/account-pages.spec.ts tests/e2e/specs/auth-flows.spec.ts --project=chromium --workers=1`

Also ensure modified files still pass lint/typecheck as applicable for the touched area (at minimum, ensure Playwright test files compile).
