---
id: continue-20260105-001500-stripe-end-to-end-payments-page
timestamp: 2026-01-05T00:15:00Z
executed: false
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Finish the Stripe payment-methods slice end-to-end so the web Payments page can list and delete saved card methods reliably, while keeping Nx checks green.

## Scope

### Web integration validation

- Confirm the web page [tejospec/apps/web/src/pages/account/payments.tsx](tejospec/apps/web/src/pages/account/payments.tsx) calls:
  - `GET /stripe/payment-methods`
  - `DELETE /stripe/payment-methods/:id`
- Confirm the Next rewrite in [tejospec/apps/web/next.config.js](tejospec/apps/web/next.config.js) proxies `/api/*` to the backend base URL normalized to `/api/v1`.

### API correctness

- Ensure the Stripe controller returns proper HTTP errors (no generic thrown `Error` for auth issues).
- Ensure `StripeService`:
  - Uses Stripe SDK when `STRIPE_SECRET_KEY` is set.
  - Returns an empty list / no-op detach when Stripe is not configured.
  - Checks ownership before detaching a payment method.

## Constraints

- Prefer Nx for checks (e.g., `nx run @tejo/web:typecheck`, `nx run @tejo/api:typecheck`).
- Keep changes minimal and localized.

## Acceptance Criteria

- `@tejo/web` typecheck passes.
- `@tejo/api` typecheck passes.
- Payments page can render with empty methods when Stripe is unconfigured (no crashes).

## Verification

- Run:
  - `pnpm nx run @tejo/web:typecheck`
  - `pnpm nx run @tejo/web:lint` (if configured)
  - `pnpm nx run @tejo/api:typecheck`
