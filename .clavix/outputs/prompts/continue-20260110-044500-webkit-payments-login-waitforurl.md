---
id: continue-20260110-044500-webkit-payments-login-waitforurl
timestamp: 2026-01-10T04:45:00Z
executed: true
originalPrompt: "continue working!"
---

# Improved Prompt

## Objective

Remove the remaining WebKit flake in `tests/e2e/specs/account-pages.spec.ts` where the Payments Page test intermittently times out inside `loginAsAdmin()` waiting for an overly strict post-login URL.

## Scope

- Update the post-login `page.waitForURL(...)` in `loginAsAdmin()` so it matches any account sub-route (e.g. `/account`, `/account/profile`, `/account/payments`, etc.), not just the exact `/account` path.
- Keep existing auth stubbing (`/api/auth/login`, `/api/auth/me`) and `data-testid`-based assertions unchanged.

## Constraints

- Prefer stable URL patterns over localized text assertions.
- Keep the change minimal and isolated to the E2E helper.
- Use Nx to run verification (do not introduce new scripts).

## Acceptance Criteria

- Re-running the WebKit Payments Page spec no longer reports a flake caused by `page.waitForURL` timing out.
- Full `@tejo/root:e2e` runs should not introduce new failures.
