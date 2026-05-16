---
id: std-20260109-240500-tejospec-e2e-checkout-flow-cart-add-to-cart-enabled
timestamp: 2026-01-10T00:05:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Make the `Checkout Flow` Playwright E2E spec reliable on the `apps/web` storefront running at `http://localhost:3003` by removing brittle assumptions around cart badge rendering and by ensuring the test adds an in-stock product.

## Scope

- Update `tejospec/tests/e2e/specs/checkout-flow.spec.ts` only.
- Do **not** change application UI/production code unless strictly necessary.

## Problem Summary

The test sometimes selects a product whose `Add to cart` button is disabled (out-of-stock), and/or relies on a conditional cart badge (`data-testid=cart-count`) that only renders when cart state is non-empty and hydration/state propagation has completed.

## Required Changes

1. **Deterministic cart state**

   - Clear persisted cart storage at test start (remove the `tejo-cart` localStorage key) so the test does not depend on prior runs.

2. **Select an enabled Add-to-Cart button**

   - Instead of using the first product card, select the first `Add to cart` button that is **not** disabled:
     - `locator('[data-testid="add-to-cart"]:not([disabled])').first()`

3. **Wait for cart state update in a robust way**

   - After clicking add-to-cart, wait until localStorage `tejo-cart` contains a `state.items` array with length > 0.
   - Avoid asserting `cart-count` visibility as a prerequisite.

4. **Validate cart is non-empty via cart page**
   - Navigate to `/cart` via `[data-testid="cart-icon"]` and assert `[data-testid="checkout-button"]` is visible.

## Constraints

- Keep assertions locale-agnostic (no Croatian text coupling).
- Keep selectors stable (`data-testid` preferred).

## Acceptance Criteria

- `STABLE_TEST_RUN=1 pnpm exec playwright test tests/e2e/specs/checkout-flow.spec.ts --project=chromium` passes reliably.
- The broader focused suite (`advanced-filters`, `checkout-flow`, `product-browsing`) passes on chromium.
