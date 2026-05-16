---
id: std-20251230-001500-continue-endgame-decision
timestamp: 2025-12-30T00:15:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue toward a “finished to the end” canonical wholesale state by choosing and executing one of two endgame paths:

- Option A (canonical-minimal): remove unfinished B2B wholesale flows from the canonical web app and keep only the catalog-request funnel.
- Option B (full B2B): migrate the missing wholesale endpoints into `apps/api` (Nest) so the canonical web B2B pages can be enabled safely.

## Current State

- Canonical `/api/v1` route alignment is fixed for `apps/api` wholesale controller.
- Canonical web now blocks unfinished B2B pages by default via middleware and feature flag.
- `pnpm verify:wholesale` and web/api typechecks are passing.

## Scope

### Option A (recommended if catalog-request is v1)

- Remove or archive these pages (and all links/components dedicated to them):
  - `apps/web/src/pages/auth/signup-b2b.tsx`
  - `apps/web/src/pages/checkout/b2b.tsx`
  - `apps/web/src/pages/wholesale/bulk-upload.tsx`
- Remove the middleware gate and the `NEXT_PUBLIC_ENABLE_B2B_WHOLESALE` flag.
- Ensure wholesale marketing pages still point to `/wholesale/catalog-request`.

### Option B (recommended if B2B checkout is required)

- Implement the minimum viable endpoint set in `apps/api` under `/api/v1/wholesale/**`:
  - `POST /wholesale/apply`
  - `POST /wholesale/checkout`
  - `POST /wholesale/validate-bulk`
  - `POST /wholesale/cart/bulk-add`
- Define auth/roles explicitly (public apply vs authenticated partner vs admin).
- Extend `scripts/verify-wholesale.mjs` to cover the newly supported flows.
- Enable the feature flag in the deployment environment.

## Constraints

- Do not delete or migrate legacy `backend/**` code without explicit approval.
- Keep verification green throughout.

## Acceptance Criteria

- One endgame path is fully implemented and documented.
- No canonical page links to unsupported endpoints.
- `pnpm verify:wholesale` passes.
