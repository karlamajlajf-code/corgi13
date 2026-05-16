---
id: continue-20260110-002200-e2e-mobile-chrome-account-nav
timestamp: 2026-01-10T00:22:00Z
executed: true
originalPrompt: "continue"
context: "Playwright default multi-project run currently fails on Mobile Chrome due to hidden desktop-only links being clicked (account-pages.spec.ts and auth-flows.spec.ts)."
---

# Improved Prompt

## Objective

Stabilize the default Playwright E2E run across **all configured projects** (including **Mobile Chrome**) for the `apps/web` storefront served at `http://localhost:3003` by fixing the remaining Mobile Chrome failure in the Account Navigation tests.

## Scope

- Update `tejospec/tests/e2e/specs/account-pages.spec.ts`:

  - In the `Account Navigation` describe block, fix back-navigation to `/account` so it works on mobile layouts.
  - Avoid clicking desktop-only hidden links (e.g., links with `hidden sm:flex`) that match `a[href$="/account"]`.

- Update `tejospec/tests/e2e/specs/auth-flows.spec.ts`:
  - Ensure navigation links (back to login / register / forgot password) click a **visible** element on mobile layouts, not the hidden desktop sidebar link.

## Constraints

- Prefer stable, layout-agnostic selectors:
  - Use Playwright visibility filtering (`:visible`) or other deterministic scoping.
  - Do **not** rely on translated link text.
- Keep behavior consistent on desktop/tablet projects.
- Minimize changes: only adjust selectors/locators needed to remove the mobile flake/failure.

## Acceptance Criteria

- Running the default suite (all projects) no longer fails due to `Account Navigation › should navigate between account sections` in **Mobile Chrome**.
- Specifically, the “Navigate back” step clicks a **visible** `/account` link on mobile (e.g., the page’s “back to account” link) rather than the hidden desktop sidebar link.
- Focused verification passes:
  - `pnpm exec playwright test tests/e2e/specs/account-pages.spec.ts --project="Mobile Chrome" --workers=1`
- No new failures are introduced in Chromium/Firefox/WebKit projects.
