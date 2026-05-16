---
id: continue-20260105-003000-stripe-add-card-elements
timestamp: 2026-01-05T00:30:00Z
executed: false
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Complete the Stripe "add card" flow end-to-end for the canonical `tejospec/` apps: API creates a SetupIntent, and the web Payments page uses Stripe Elements to confirm the setup and then refreshes the saved payment-method list.

## Scope

### API (NestJS)

- Ensure these authenticated endpoints exist and behave safely:
  - `GET /stripe/payment-methods` (list saved card methods)
  - `DELETE /stripe/payment-methods/:id` (detach, with ownership safety)
  - `POST /stripe/setup-intent` (create SetupIntent; return `{ clientSecret }`)
- When `STRIPE_SECRET_KEY` is missing or Stripe errors occur, keep MVP-safe behavior:
  - list returns `[]`
  - detach is a no-op
  - setup-intent returns a 503 error (Service Unavailable)

### Web (Next.js Pages Router)

- Add Stripe client dependencies to `@tejo/web`:
  - `@stripe/stripe-js`
  - `@stripe/react-stripe-js`
- Update the Payments page modal [tejospec/apps/web/src/pages/account/payments.tsx](tejospec/apps/web/src/pages/account/payments.tsx) to:
  - Let the user choose "card" and show a client-only Stripe Elements form
  - Call `POST /stripe/setup-intent` to obtain `clientSecret`
  - Use `stripe.confirmCardSetup(clientSecret, { payment_method: { card } })`
  - On success: close modal and refresh `GET /stripe/payment-methods`
  - Handle missing `NEXT_PUBLIC_STRIPE_PUBLISHABLE_KEY` gracefully (show a friendly error, no crash)

## Constraints

- Keep changes minimal and localized.
- Use Nx for verification targets.

## Acceptance Criteria

- `pnpm nx run @tejo/api:typecheck` passes.
- `pnpm nx run @tejo/web:typecheck` passes.
- `pnpm nx run @tejo/web:lint` passes.
- Payments page can add a card when configured, and shows empty state safely when unconfigured.

## Verification

- Run Nx targets:
  - `@tejo/api:typecheck`
  - `@tejo/web:typecheck`
  - `@tejo/web:lint`
