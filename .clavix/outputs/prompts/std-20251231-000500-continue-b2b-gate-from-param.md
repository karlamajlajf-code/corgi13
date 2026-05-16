---
id: std-20251231-000500-continue-b2b-gate-from-param
timestamp: 2025-12-31T00:05:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue hardening the canonical wholesale regression checks by verifying not only that B2B pages redirect to the catalog request page, but also that the redirect includes a stable `from=<pathname>` hint.

## Scope

- Update `tejospec/scripts/smoke-wholesale.mjs` B2B gate checks (proxy mode only):
  - For `/auth/signup-b2b`, `/checkout/b2b`, `/wholesale/bulk-upload`, assert redirect location:
    - `pathname === /wholesale/catalog-request`
    - `blocked=b2b`
    - `from` equals the blocked pathname
  - Continue to be non-failing by default unless `SMOKE_B2B_GATE_REQUIRED=1`.

## Acceptance Criteria

- `pnpm verify:wholesale` passes.
- Proxy mode outputs `OK: B2B gate ok (...)` and the check includes `from` correctness.
