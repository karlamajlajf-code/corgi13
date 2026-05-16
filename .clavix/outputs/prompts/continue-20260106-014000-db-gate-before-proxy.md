---
id: continue-20260106-014000-db-gate-before-proxy
timestamp: 2026-01-06T01:40:00Z
executed: true
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Reduce wasted time/noise when the database is down by making the live+Stripe wrapper detect DB-down immediately after the API health endpoint is reachable, before waiting for the Next.js proxy health and before running any pre-stripe steps.

## Scope

- Update `tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs`:
  - Move the existing DB status gate (`services.database !== 'up'`) to run right after `waitForOk('API health', ...)`.
  - Keep a fallback DB check later only if the early health JSON could not be read/parsed.
- Keep current UX: exit code `2` and the existing actionable DB/Docker hint output.

## Constraints

- No runtime API changes.
- No new destructive behavior.
- Keep timeouts configurable via existing env vars (`SMOKE_HEALTH_TIMEOUT_MS`).

## Acceptance Criteria

- With DB down, `node ./scripts/verify-live-with-stripe-proxy-auto.mjs --start-live --takeover-ports --skip-flow`:
  - exits with code `2`
  - prints the DB hint
  - does **not** print `OK: waiting for web proxy health` (since it fails earlier)
  - does not run `pnpm smoke:live` / `pnpm verify:live` and does not invoke Stripe smoke.
