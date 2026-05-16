# Implementation Plan

**Project**: auth-security-tejospec
**Generated**: 2025-12-19

## Technical Context & Standards

### Detected Stack & Patterns

- **Framework**: Node.js + Express (TypeScript)
- **API**: Express routers mounted in `tejospec/backend/src/app.ts`
- **Validation**: `zod` schemas in route handlers
- **Auth**: JWT access tokens in `Authorization: Bearer ...`; refresh tokens in HTTP-only cookies; session revocation via Prisma `Session`
- **CSRF**: double-submit cookie, applied selectively for cookie-auth flows (`tejospec/backend/src/middleware/csrf.ts`)
- **DB/ORM**: Prisma + PostgreSQL (`tejospec/backend/prisma/schema.prisma`)
- **Testing**: Jest + ts-jest (`tejospec/backend/jest.config.ts`), integration tests under `tejospec/backend/tests/integration`
- **Logging**: `pino` via `tejospec/backend/src/infra/logger`
- **Workspace**: pnpm workspaces + Nx present (`tejospec/package.json`, `tejospec/nx.json`)

**Primary Spec Source**: `.clavix/outputs/prompts/std-20251218-000001-auth.md` (executed auth checklist)

---

## Phase 1: Spec Alignment & Proof (Checklist Closure)

- [x] **Reconcile auth checklist status vs code** (ref: Prompt "Implementation status in tejospec")
      Task ID: phase-1-spec-alignment-01

  > **Implementation**: Update `.clavix/outputs/prompts/std-20251218-000001-auth.md`.
  > **Details**: Verify and then update the unchecked items:
  >
  > - Mark **Database: PostgreSQL with Prisma ORM** as complete if `provider = "postgresql"` and local dev uses Postgres by default.
  > - Decide the status of **All sensitive routes are protected** based on a concrete route inventory (Phase 2).
  >   Add an "Evidence" subsection linking to the exact files/tests that prove each checkbox.

- [x] **Create a route inventory doc (public vs customer vs admin)** (ref: Prompt "All sensitive routes are protected")
      Task ID: phase-1-spec-alignment-02
  > **Implementation**: Create `tejospec/backend/docs/security/route-access-matrix.md`.
  > **Details**: Enumerate every router mounted in `tejospec/backend/src/app.ts` and classify endpoints:
  >
  > - Public read-only
  > - Customer-auth required (`requireCustomerAuth`)
  > - Admin-auth required (`requireAdmin` / `requireRole`)
  > - Webhooks/dev-only
  >   For each, note the enforcement middleware and any PII exposure.

---

## Phase 2: Sensitive Route Protection (Gap Audit + Hardening)

- [x] **Audit customer-facing mutation routes for auth** (ref: Prompt "All sensitive routes are protected")
      Task ID: phase-2-sensitive-routes-01

  > **Implementation**: Review and update route files under `tejospec/backend/src/api/routes/`.
  > **Details**: For each of these routers, confirm writes are protected and reads do not expose cross-user data:
  >
  > - `cart.ts`, `wishlist.ts`, `orders.ts`, `upload.ts`, `media.ts`, `reviews.ts`
  >   Use `requireCustomerAuth` for customer-specific operations and ensure guest flows are token-based and time-limited.

- [x] **Audit all `/api/admin/**` routes for strict admin auth\*\* (ref: Prompt Security Requirements)
      Task ID: phase-2-sensitive-routes-02

  > **Implementation**: Review routers mounted under `/api/admin` in `tejospec/backend/src/app.ts`.
  > **Details**: Ensure every admin endpoint requires `requireAdmin` (or stronger role checks via `requireRole`) and that no admin router is mounted without internal auth checks.

- [x] **Lock down PII reads on “public” endpoints** (ref: Prompt "All sensitive routes are protected")
      Task ID: phase-2-sensitive-routes-03
  > **Implementation**: Update any endpoints that accept an email/id in the path or query.
  > **Details**: No endpoint should allow arbitrary email-based lookups without one of:
  >
  > - authenticated customer ownership checks, or
  > - a short-lived signed token bound to the identity being accessed.

---

## Phase 3: Time-Limited Signed Tokens for Guest-Like Sensitive Flows

- [x] **Document newsletter preferences token contract** (ref: Prompt Security Requirements)
      Task ID: phase-3-signed-tokens-01

  > **Implementation**: Update `tejospec/openapi/openapi.yaml`.
  > **Details**: Add/confirm endpoints and security semantics:
  >
  > - `POST /api/newsletter/preferences/token` (generic success response; token returned only in test)
  > - `GET|PATCH /api/newsletter/preferences/{email}?token=...` (token required)
  > - `GET|PATCH /api/newsletter/preferences` (customer auth)
  >   Ensure the token query parameter is documented and response schemas match `tejospec/backend/src/api/routes/newsletter.ts`.

- [x] **Document guest payment intent token contract** (ref: Prompt "All sensitive routes are protected")
      Task ID: phase-3-signed-tokens-02

  > **Implementation**: Update `tejospec/openapi/openapi.yaml`.
  > **Details**: Document `POST /api/payment/intents` as:
  >
  > - Preferred flow: `{ orderId }` + `x-guest-token` header (token scoped to `{orderId,email}`)
  > - Legacy non-prod flow (if kept): `{ amount, currency }` only when not production
  >   Document `PATCH /api/payment/intents/{id}/status` as admin-only.

- [x] **Regenerate frontend API client from OpenAPI** (ref: Prompt "Expected Output")
      Task ID: phase-3-signed-tokens-03
  > **Implementation**: Update `tejospec/frontend/web/src/generated/api/**` via your existing OpenAPI generation pipeline.
  > **Details**: Ensure the generated `PaymentService` and newsletter client calls match the token-based APIs (header/query params). If the generator cannot express `x-guest-token`, add a thin wrapper in `tejospec/frontend/web/src/services/*` that sets headers explicitly.

---

## Phase 4: Tests & Coverage (Proof of Correctness)

- [x] **Add integration tests for “sensitive route requires auth”** (ref: Prompt Success Criteria)
      Task ID: phase-4-tests-01

  > **Implementation**: Create `tejospec/backend/tests/integration/sensitive_routes_auth.test.ts`.
  > **Details**: Add a small set of regression checks (401/403) for representative routes:
  >
  > - customer-only endpoints without auth
  > - admin-only endpoints without auth
  >   Keep the test deterministic (create its own fixtures and clean up).

- [x] **Confirm unit coverage thresholds for auth modules** (ref: Prompt "Unit tests with >80% coverage")
      Task ID: phase-4-tests-02
  > **Implementation**: Use `tejospec/backend/jest.coverage.config.ts`.
  > **Details**: Run unit coverage (not integration) and ensure it stays above thresholds for:
  >
  > - `src/api/routes/auth.ts`
  > - `src/middleware/customerAuth.ts`
  > - `src/middleware/csrf.ts`
  > - `src/utils/password.ts`

---

## Phase 5: Operational Readiness

- [x] **Environment + secrets hardening for token secrets** (ref: Prompt Security Requirements)
      Task ID: phase-5-ops-01
  > **Implementation**: Update `tejospec/backend/README.md` and `.env.example` equivalents (if present).
  > **Details**: Document required secrets:
  >
  > - `JWT_SECRET`, `REFRESH_SECRET`
  > - `NEWSLETTER_PREFS_TOKEN_SECRET` (optional override)
  > - `GUEST_CHECKOUT_TOKEN_SECRET` (optional override)
  >   Include guidance for production rotation and minimum entropy.

---

Generated by Clavix /clavix-plan.
