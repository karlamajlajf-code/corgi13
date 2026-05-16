---
id: continue-20260105-000000-phase4-stripe-payment-methods-fix-api-typecheck
timestamp: 2026-01-05T00:00:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue Phase 4 hardening in the canonical `tejospec/` Nx workspace: keep the Stripe payment-method endpoints functional when Stripe is configured, and restore green API TypeScript typecheck.

## Scope

### API / Stripe

- Ensure `StripeService` uses the Stripe SDK when `STRIPE_SECRET_KEY` is set.
- Store or retrieve a Stripe customer id for the authenticated user (use existing DB model with minimal schema churn; prefer `User.metadata.stripeCustomerId` if available).
- Implement:
  - `GET /stripe/payment-methods`: list saved card payment methods for the authenticated user.
  - `DELETE /stripe/payment-methods/:id`: detach a payment method, with ownership safety checks.
- Maintain MVP-safe behavior when Stripe is not configured or on Stripe errors (return empty list / no-op detach; do not crash the UI).

### API / Search typecheck fix

- Fix the TypeScript error in `apps/api/src/modules/search/search.service.ts` where `ordered.map(...)` treats items as possibly `undefined`.
- Use explicit type narrowing (type predicate / non-null narrowing) so `toSearchProductDto` only receives valid products.

## Constraints

- Follow repo guidance: prefer running checks via Nx (`nx run ...`) instead of calling underlying tools directly.
- Keep changes minimal and localized (no broad refactors).
- Do not introduce breaking API surface changes.

## Acceptance Criteria

- API compiles: `nx run <apiProject>:typecheck` passes.
- Stripe module behavior:
  - When `STRIPE_SECRET_KEY` is unset: endpoints remain safe (empty list; detach no-op).
  - When set: endpoints use Stripe SDK and do not throw for common cases.
  - Detach does not detach a payment method owned by a different Stripe customer.

## Verification

- Run Nx API typecheck.
- If lint is configured for API via Nx, run Nx API lint.
