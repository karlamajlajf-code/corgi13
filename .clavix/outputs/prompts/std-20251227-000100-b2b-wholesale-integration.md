---
id: std-20251227-000100-b2b-wholesale-integration
timestamp: 2025-12-27T00:01:00Z
executed: false
originalPrompt: "Continue Phase 3 B2B wholesale end-to-end integration: fix B2B signup/apply payload mismatch, make wholesale dashboard/me work without hacks, and implement missing wholesale checkout endpoint used by the Next.js pages."
---

# Improved Prompt

## Objective

Make the B2B wholesale flow work end-to-end between the Next.js pages (`apps/web`) and the Express API (`tejospec/backend`) by resolving remaining integration mismatches and missing endpoints.

## Scope

1. **Wholesale application (B2B signup) compatibility**

   - Update `POST /api/wholesale/apply` to accept the payload sent by `apps/web/src/pages/auth/signup-b2b.tsx`.
   - Support `multipart/form-data` (FormData) with optional `businessLicenseFile` upload.
   - Also support JSON for programmatic calls (backwards-compatible).
   - If no `userId` is supplied and an account does not exist, create the user from `email/password/name/phone` before creating/upserting the PartnerProfile.

2. **Customer identity resolution for wholesale endpoints**

   - Ensure wholesale endpoints can resolve the customer ID via existing customer auth (`requireCustomerAuth`) when present.
   - Keep a temporary fallback for development (`userId` query param) without breaking production auth flows.

3. **Wholesale checkout endpoint**

   - Implement `POST /api/wholesale/checkout` to support `apps/web/src/pages/checkout/b2b.tsx`.
   - Verify the user is an approved wholesale partner.
   - Load wholesale cart totals using `wholesaleService.getB2BCart()` and enforce minimum order amount.
   - Create an `Order` + `OrderItem[]` from the cart using wholesale pricing, store checkout form fields in `shippingAddress`/`billingAddress`/`metadata`, and clear the cart.

4. **Auth middleware correctness**
   - Update `tejospec/backend/src/middleware/customerAuth.ts` to reflect the updated `User` model fields (`isWholesaleApproved`, `wholesaleTier`) and avoid stale comments.

## Constraints

- Keep changes minimal and backwards-compatible.
- Don’t require DB migrations to run as part of this change (but code should be correct once `DATABASE_URL` is configured).
- Use existing repo patterns (zod validation, prisma, multer patterns from upload/media routes).

## Acceptance Criteria

- B2B signup form can successfully submit to `POST /api/wholesale/apply` using FormData.
- `GET /api/wholesale/me` works when called with a valid customer auth token, and also works with `?userId=` fallback.
- `POST /api/wholesale/checkout` returns `{ orderId, orderNumber }` on success and errors with a consistent `{ error, message }` shape.
- No new TypeScript errors in touched files.

## Suggested Verification

- Run backend unit/type checks (or at least TS compile/lint for touched files).
- Manually smoke test:
  1. Submit B2B application
  2. Approve in admin
  3. Load wholesale dashboard
  4. Bulk upload → add to cart
  5. Checkout B2B → creates order
