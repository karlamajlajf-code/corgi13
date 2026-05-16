---
id: std-20251230-004500-continue-verify-b2b-gate
timestamp: 2025-12-30T00:45:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue hardening the canonical wholesale experience by adding a regression check to the canonical wholesale verification script, ensuring unfinished B2B pages remain blocked by default.

## Scope

- Update `tejospec/scripts/verify-wholesale.mjs`:
  - In **proxy mode** (Next.js web base URL), request these paths with redirects disabled:
    - `/auth/signup-b2b`
    - `/checkout/b2b`
    - `/wholesale/bulk-upload`
  - Assert they return an HTTP redirect (301/302/303/307/308) to `/wholesale/catalog-request` containing `blocked=b2b`.
  - If the server returns 200 (meaning B2B may be enabled), do not fail by default; print a clear `SKIP` line unless an explicit env forces gating assertion.

## Constraints

- Do not add new API endpoints.
- Keep `pnpm verify:wholesale` focused and fast.

## Acceptance Criteria

- `pnpm verify:wholesale` still passes.
- When B2B is disabled (default), the script reports `OK` for the three B2B route gate checks.
