---
id: continue-20260110-052500-remove-locale-dependent-entrance-wait
timestamp: 2026-01-10T05:25:00Z
executed: true
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Remove remaining locale-coupled E2E waits that depend on the Croatian aria-label `Dobrodosli.` for the EntranceLoader overlay, since the overlay is already disabled globally via the shared E2E fixture.

## Scope

- Update `tests/e2e/specs/account-pages.spec.ts` and `tests/e2e/specs/auth-flows.spec.ts` to stop querying `[aria-label="Dobrodosli."]`.
- Replace that logic with locale-agnostic readiness checks:
  - wait for the relevant form inputs/testids to be visible and enabled (e.g., `login-email`, `login-password`, `login-submit`) before interacting.
- Keep existing stubbing and `data-testid`-based selectors unchanged.

## Constraints

- Do not introduce assertions that depend on localized text.
- Keep the fix minimal and limited to test code.
- Verify via Nx (`pnpm nx run @tejo/root:e2e`).

## Acceptance Criteria

- No E2E spec references `Dobrodosli` aria-label anymore.
- The default multi-project E2E suite remains green (at least with `--max-failures=1`), and key affected specs pass on WebKit.
