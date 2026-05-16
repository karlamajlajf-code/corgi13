---
id: std-20260109-235900-tejospec-e2e-gate-legacy-guest-and-immersive
timestamp: 2026-01-09T23:59:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Keep the default Playwright E2E run stable against the **apps/web** storefront served at **[http://localhost:3003](http://localhost:3003)** by skipping E2E specs that target a different frontend (e.g. `/products`, guest checkout UI, immersive components, or port 3000).

## Scope

1. **Gate legacy/mismatched E2E specs** so they are **opt-in** via an environment variable, while remaining available for manual runs.

   - Gate these specs (skip by default):
     - `tejospec/tests/e2e/specs/guest-checkout-simple.spec.ts`
     - `tejospec/tests/e2e/specs/guest-checkout.spec.ts`
     - `tejospec/tests/e2e/specs/immersive-frontend.spec.ts`
     - `tejospec/tests/e2e/specs/immersive-quick-check.spec.ts`

2. **Use consistent gating pattern**

   - Follow the existing admin gate style (like `E2E_ADMIN_UI=true`).
   - Introduce `E2E_LEGACY_UI=true` (or similarly named) to enable the gated specs.

3. **Verification**
   - Ensure Playwright still discovers tests (`--list`) and that the default suite no longer attempts to run the gated legacy tests.

## Constraints

- Do not change runtime application behavior (only adjust E2E spec gating).
- Keep the default suite aligned to `PLAYWRIGHT_BASE_URL` (defaults to `http://localhost:3003`).

## Acceptance Criteria

- Gated specs are skipped unless the env var is enabled.
- `pnpm exec playwright test --list` shows the gated tests as skipped (or otherwise non-running) by default.
