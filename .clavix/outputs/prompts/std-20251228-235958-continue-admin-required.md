---
id: std-20251228-235958-continue-admin-required
timestamp: 2025-12-28T23:59:58Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Make the wholesale smoke validation usable in CI by allowing the admin inbox checks to be required (fail if missing) rather than always optional.

## Scope

- Add an env flag (e.g., `SMOKE_ADMIN_REQUIRED=1`) to `tejospec/scripts/smoke-wholesale.mjs`.
- When enabled, the script must fail if neither `SMOKE_ADMIN_TOKEN` nor `SMOKE_ADMIN_EMAIL`/`SMOKE_ADMIN_PASSWORD` is provided.
- Update docs to describe the new flag and common usage patterns.

## Constraints

- Do not add or print real credentials.
- Keep default behavior unchanged (admin checks remain optional unless explicitly required).

## Acceptance Criteria

- `SMOKE_ADMIN_REQUIRED=1 pnpm smoke:wholesale` exits non-zero if admin auth inputs are missing.
- Existing flows (no admin envs set) still PASS when `SMOKE_ADMIN_REQUIRED` is unset/0.
- `node --check tejospec/scripts/smoke-wholesale.mjs` succeeds.
