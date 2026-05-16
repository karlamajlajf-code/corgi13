---
id: std-20260103-162300-continue-fix-api-lint-any
timestamp: 2026-01-03T16:23:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue to a fully clean canonical state by removing the remaining `@typescript-eslint/no-explicit-any` warnings in the canonical API controllers, without changing product behavior.

## Scope

- Fix only the files reported by `nx run @tejo/api:lint` as using `any`:
  - `apps/api/src/modules/cart/cart.controller.ts`
  - `apps/api/src/modules/orders/orders.controller.ts`
  - `apps/api/src/modules/reviews/reviews.controller.ts`
  - `apps/api/src/modules/wishlist/wishlist.controller.ts`
- Prefer proper typing (`Request`, DTOs, inferred return types) over disabling eslint rules.

## Constraints

- No feature changes and no endpoint behavior changes.
- No modifications to legacy folders (`backend/`, `frontend/`, `archive/`).
- Keep changes minimal and aligned with existing NestJS patterns in the repo.

## Acceptance Criteria

- `pnpm exec nx run @tejo/api:lint` has **zero** `no-explicit-any` warnings.
- `pnpm exec nx run @tejo/api:typecheck` succeeds.
- `pnpm verify:wholesale:strict` still passes.
