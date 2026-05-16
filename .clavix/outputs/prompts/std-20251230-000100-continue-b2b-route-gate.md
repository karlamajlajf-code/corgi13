---
id: std-20251230-000100-continue-b2b-route-gate
timestamp: 2025-12-30T00:01:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue to a clean, stable “canonical-only” end state by preventing users from accessing unfinished B2B wholesale flows in the canonical web app, while keeping the canonical wholesale catalog-request flow and verification green.

## Scope

- Add a canonical web guard (feature flag) that disables the unfinished B2B pages by default:
  - `/auth/signup-b2b`
  - `/checkout/b2b`
  - `/wholesale/bulk-upload`
- Redirect blocked routes to the supported wholesale entrypoint (catalog request) with a simple notice param.
- Document the feature flag and current status in the wholesale legacy→canonical mapping doc.

## Constraints

- Do not implement/migrate the legacy Express wholesale endpoints (`apply`, `checkout`, `validate-bulk`, `cart/bulk-add`) in this step.
- Do not change the canonical health contract or the wholesale catalog-request endpoints.
- Keep changes minimal and reversible.

## Acceptance Criteria

- With the flag disabled (default), the above routes redirect safely to `/wholesale/catalog-request`.
- With the flag enabled, routes behave as before.
- `pnpm verify:wholesale` continues to pass.
