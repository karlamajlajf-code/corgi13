---
id: continue-20260105-010000-stripe-smoke-script-and-lint-cleanup
timestamp: 2026-01-05T01:00:00Z
executed: false
originalPrompt: "Continue Working"
---

# Improved Prompt

## Objective

Continue the Stripe payments hardening work by adding a lightweight, repeatable smoke script for the Stripe API endpoints and by removing the remaining `@tejo/api` ESLint warning (no behavior changes).

## Scope

1. Add a local developer smoke script that can be run without committing secrets. The script should:

- Call authenticated API endpoints:
  - `GET /stripe/payment-methods`
  - `POST /stripe/setup-intent`
- Be configurable via env vars (API base URL + bearer token).
- Fail fast with clear messages when required env vars are missing.
- Gracefully handle "Stripe not configured" responses (e.g., 503) and print a clear note.

1. Remove the remaining `@typescript-eslint/no-explicit-any` warning in the API search service by tightening types (prefer `unknown` + safe narrowing) without changing runtime behavior.

## Constraints

- Do not add or commit any secrets/keys.
- Keep changes minimal and consistent with existing repo patterns.
- Keep Nx checks green.

## Acceptance Criteria

- A new script exists under `tejospec/` (prefer `scripts/`) and is runnable via a package.json script (or documented invocation).
- `pnpm nx run @tejo/api:lint` is clean (no remaining `no-explicit-any` warnings from SearchService).
- `pnpm nx run @tejo/api:typecheck`, `pnpm nx run @tejo/web:typecheck`, and `pnpm nx run @tejo/web:lint` succeed.

## Suggested Verification Commands

- `cd tejospec && pnpm nx run @tejo/api:lint`
- `cd tejospec && pnpm nx run @tejo/api:typecheck`
- `cd tejospec && pnpm nx run @tejo/web:lint`
- `cd tejospec && pnpm nx run @tejo/web:typecheck`
