---
id: std-20260109-233000-tejospec-e2e-checkout-route-testids-and-legacy-skips
timestamp: 2026-01-09T23:30:00Z
executed: false
originalPrompt: "Continue stabilizing the Playwright E2E suite (next failure: checkout-flow.spec.ts still uses /products and missing test IDs)."
---

# Improved Prompt

## Objective

Unblock and stabilize the Playwright E2E suite running against `http://localhost:3003` by:

1. Aligning checkout-related E2E tests to the _actual_ storefront routes and UI in `tejospec/apps/web` (not `/products`).
2. Adding minimal, stable `data-testid` hooks in the web UI to support locale-agnostic and responsive-safe selectors.
3. Preventing known-out-of-scope legacy E2E specs (guest-checkout + immersive widgets) from failing the default suite run.

## Scope

### Update E2E specs

- Fix `tejospec/tests/e2e/specs/checkout-flow.spec.ts` to:

  - Navigate to `/shop` (not `/products`).
  - Add first product to cart by scoping `add-to-cart` inside the first `product-card`.
  - Navigate to cart via `cart-icon`.
  - Proceed to checkout via a stable `checkout-button` selector.
  - Complete the checkout stepper (shipping → payment → review → place order) using `data-testid` selectors.
  - Verify redirect to `/checkout/success` and a stable `order-success` marker.

- Fix `tejospec/tests/e2e/specs/product-browsing.spec.ts` to:

  - Use `/shop`.
  - Use stable selectors (`product-card`, `sort-select`, `product-price`, filter IDs) and avoid language-coupled text assertions.

- Update any “quick visual audit” page lists that still include `/products` to use `/shop`.

### Add / adjust UI test IDs (minimal + non-invasive)

- `tejospec/apps/web/src/components/product/ProductCard.tsx`

  - Add `data-testid="add-to-cart"` on the add-to-cart button.
  - Add `data-testid="product-price"` on the main displayed price.

- `tejospec/apps/web/src/components/layout/Header.tsx`

  - Add `data-testid="cart-icon"` on the cart link.
  - Add `data-testid="cart-count"` on the cart badge (when present).

- `tejospec/apps/web/src/pages/cart.tsx`

  - Add `data-testid="checkout-button"` on the `/checkout` link.

- `tejospec/apps/web/src/pages/checkout.tsx`

  - Add `data-testid` hooks for required shipping + payment inputs and step navigation buttons:
    - Shipping: `checkout-email`, `checkout-firstName`, `checkout-lastName`, `checkout-address`, `checkout-city`, `checkout-postalCode` (and optionally phone).
    - Actions: `continue-to-payment`, `review-order`, `place-order`.

- `tejospec/apps/web/src/pages/checkout/success.tsx`
  - Add a stable marker `data-testid="order-success"`.

### Skip legacy/out-of-scope specs by default

- Mark guest-checkout specs as opt-in (e.g., require `E2E_GUEST_CHECKOUT=true`).
- Mark immersive widget specs as opt-in (e.g., require `E2E_IMMERSIVE=true`).

## Constraints

- Do not use localized visible text as primary selectors in E2E; prefer `data-testid`.
- Ensure selectors work on Mobile Chrome as well as desktop (avoid strict-mode collisions by scoping).
- Keep UI changes minimal (attributes only) with no behavior changes unless needed to satisfy test coverage.

## Acceptance Criteria

- `checkout-flow.spec.ts` passes across all Playwright projects.
- `product-browsing.spec.ts` passes across all Playwright projects.
- Default full-suite run no longer fails due to `/products` 404 or missing selectors in checkout-flow.
- Guest checkout + immersive specs are skipped unless explicitly enabled via env vars.

## Verification Plan

- Run the narrowest checks first:
  - `nx run @tejo/root:e2e-ci--tests/e2e/specs/checkout-flow.spec.ts`
  - `nx run @tejo/root:e2e-ci--tests/e2e/specs/product-browsing.spec.ts`
- Then run the full E2E target until first failure to identify the next deterministic blocker.
