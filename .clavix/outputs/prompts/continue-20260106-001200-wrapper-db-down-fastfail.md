---
id: continue-20260106-001200-wrapper-db-down-fastfail
timestamp: 2026-01-06T00:12:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue hardening the Stripe auto verification workflow so it fails fast with actionable diagnostics when the DB/Docker prerequisite isn’t available, especially in `--start-live` + `--skip-flow` flows.

## Scope

1. Update [tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs](tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs):

   - After services are up (health endpoints OK), fetch the API health JSON and detect `services.database`.
   - If the DB is not `up`, exit early (code 2) with a clear message explaining:
     - Stripe smoke requires DB (for auth/user flow).
     - How to bring DB up (`pnpm db:up`, migrate/seed).
     - On Windows, likely cause is Docker Desktop not running.
   - Improve `--skip-flow` help text to clarify it does not remove the DB requirement for Stripe smoke.

2. Verify behavior:
   - `--print-plan` remains stable.
   - A `--start-live --takeover-ports --skip-flow` run exits quickly with the DB-down diagnostic (no nested start-api/start-web).

## Constraints

- Defaults remain safe (no new destructive actions).
- Do not print secrets.
- Keep Windows compatibility.

## Acceptance Criteria

- When DB is down, wrapper exits 2 with a clear next-step message.
- Existing Nx lint remains green.
