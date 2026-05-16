---
id: std-20260109-235500-tejospec-e2e-checkout-flow-and-product-browsing-fix
timestamp: 2026-01-09T23:55:00Z
executed: true
originalPrompt: "Continue stabilizing Playwright E2E by fixing checkout/product browsing tests and adding any missing stable data-testid hooks (avoid locale-coupled assertions)."
---

# Improved Prompt

## Objective

Stabilize the Playwright E2E suite for the **apps/web** storefront served at **[http://localhost:3003](http://localhost:3003)** by aligning tests to the actual routes and DOM structure, and by using stable `data-testid` selectors instead of translated text.

## Scope

1. **Fix checkout page selectors**

   - Ensure checkout stepper navigation buttons have stable selectors:
     - `data-testid="continue-to-payment"` on the Shipping step submit button
     - `data-testid="review-order"` on the Payment step submit button
   - Ensure checkout inputs have stable `data-testid`s for:
     - Shipping: `checkout-email`, `checkout-phone`, `checkout-firstName`, `checkout-lastName`, `checkout-address`, `checkout-apartment`, `checkout-postalCode`, `checkout-city`
     - Payment: `checkout-cardNumber`, `checkout-cardName`, `checkout-cardExpiry`, `checkout-cardCvv`

2. **Add missing storefront test hooks**

   - Add `data-testid="sort-select"` to the `/shop` sort dropdown.
   - Add `data-testid="order-success"` to the `/checkout/success` page for success assertions.

3. **Update failing Playwright specs to match apps/web**

   - Update `tejospec/tests/e2e/specs/checkout-flow.spec.ts`:

     - Use `/shop` (not `/products`).
     - Add product to cart by scoping `add-to-cart` to the first `product-card`.
     - Assert cart update via `cart-count` (not translated toast text).
     - Proceed via `cart-icon` → `checkout-button`.
     - Complete the checkout stepper using the new checkout testids.
     - Assert redirect to `/checkout/success` and `order-success` visible.
     - Remove any "guest checkout" assumptions.

   - Update `tejospec/tests/e2e/specs/product-browsing.spec.ts`:
     - Use `/shop`.
     - Remove search assertions if the UI has no search input on `/shop`.
     - Validate sorting via `sort-select` and compare parsed `product-price` values.
     - Validate a category filter using `filter-category-*` (desktop-only).

## Constraints

- Do not rely on translated text (`getByText(...)`) for assertions.
- Prefer `data-testid` and scoped locators to avoid responsive duplicates.
- Keep changes minimal and selector-driven (no product/checkout UX redesign).
- Follow Nx guidance: verify changes using `nx run` targets when possible.

## Acceptance Criteria

- `apps/web` compiles (TypeScript + lint).
- `checkout-flow.spec.ts` and `product-browsing.spec.ts` no longer reference `/products`.
- Specs use stable `data-testid` selectors and do not assert on localized text.
- Checkout flow assertion passes by detecting `/checkout/success` and `[data-testid="order-success"]`.
