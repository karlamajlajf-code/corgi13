---
id: std-20260109-220000-tejospec-fix-advanced-filters-shop-route-and-testids
timestamp: 2026-01-09T22:00:00Z
executed: true
originalPrompt: "continue working: unblock Playwright suite after admin gating; advanced-filters spec fails because /products is 404 on baseURL http://localhost:3003"
---

# Improved Prompt

## Objective

Unblock the next deterministic Playwright E2E failure by updating the advanced filters E2E spec to match the actual web app routes and DOM, using stable selectors that are not coupled to any specific locale.

## Scope

- Fix `tests/e2e/specs/advanced-filters.spec.ts` so it no longer navigates to a non-existent `/products` route.
- Align the spec with the real product listing page (`/shop`).
- Add stable `data-testid` hooks to the Shop page filter controls and ProductCard component so E2E tests avoid brittle text locators and avoid coupling to Croatian/German/English translations.
- Re-run the smallest relevant Playwright target to confirm the spec passes and that the suite can proceed to the next failure.

## Constraints

- Avoid text-based selectors that depend on the active locale or on translation keys being hydrated.
- Prefer `data-testid` + `getByTestId` locators.
- Keep changes minimal and non-invasive (no behavioral changes to production UX).

## Acceptance Criteria

- `/shop` is used for product listing navigation in E2E tests (no `/products` 404).
- Product cards are detectable via `data-testid="product-card"`.
- Filter controls are targetable via test IDs (category, brand, price range, quick filters, clear-all, mobile drawer toggle/close).
- `tests/e2e/specs/advanced-filters.spec.ts` passes when run in isolation.
- A follow-up Playwright run progresses past the advanced-filters spec (or shows the next failure).
