---
id: continue-20260104-100-phase4-next-step-stripe-vs-recs
createdAt: 2026-01-04T00:00:00Z
mode: improve
executed: false
source: conversation-summary
---

# Improved Prompt — Phase 4 next step (pick one track)

## Objective

Continue Phase 4 hardening work in the canonical Nx workspace rooted at `tejospec/`, keeping Nx/pnpm checks green.

## Context (current state)

- Phase 4 baseline RBAC exists (roles decorator + guard; admin-only endpoints standardized).
- Recent MVP gap-closure work focused on Blog hardening + wiring, Search response shape alignment, and adding minimal Stripe payment-method endpoints.
- Validation previously passed via Nx targets (API lint/tests, web typecheck).

## Decision needed (choose one priority)

Pick ONE of these to implement next:

### Option A — Real Stripe payment-method integration (recommended if Payments is blocking UX)

Implement real Stripe-backed behavior for:

- `GET /stripe/payment-methods`
- `DELETE /stripe/payment-methods/:id`

Requirements:

- Add Stripe SDK (server-side only) and configure via env.
- Map authenticated user to Stripe customer (create if missing; persist mapping in Prisma).
- List payment methods from Stripe, return stable DTO to web.
- Detach payment method safely (ensure it belongs to the customer).
- Proper error handling (401/403/404/409/5xx) and idempotency where reasonable.
- Add focused tests (unit or e2e-style controller/service tests) if test harness exists.

### Option B — Recommendations / search hardening (recommended if browse experience is priority)

- Audit web usage of recommendation endpoints and any contract mismatches.
- Ensure endpoints are protected appropriately (public vs auth vs admin).
- Align API response shapes with web expectations.
- Add minimal guardrails (input validation, pagination limits, safe defaults).

## Constraints

- Work only within the canonical monorepo under `tejospec/`.
- Avoid legacy folders unless explicitly requested.
- Keep changes minimal and consistent with existing patterns.

## Verification

Run the narrowest relevant checks and keep them green:

- `pnpm -C tejospec nx lint api`
- `pnpm -C tejospec nx test api`
- `pnpm -C tejospec nx run web:typecheck`

## Acceptance criteria

- Selected option is implemented end-to-end.
- No breaking changes to existing public routes without coordinating updates.
- Repository checks remain green.
