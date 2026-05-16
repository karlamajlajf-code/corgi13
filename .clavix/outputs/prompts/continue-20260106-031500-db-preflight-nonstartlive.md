---
id: continue-20260106-031500-db-preflight-nonstartlive
timestamp: 2026-01-06T03:15:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

When the user runs the live+Stripe proxy wrapper _without_ `--start-live` (services already running), fail fast with the same DB/Docker hint if the API health endpoint reports `services.database !== 'up'`, before running `pnpm verify:live`/`pnpm smoke:live` or any Stripe smoke.

## Scope

- Update `tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs`:
  - Add a non-invasive preflight: if `API_HEALTH_URL` is reachable and returns JSON with `services.database`, treat non-`up` as a hard prerequisite and exit code `2` with `printDbDownHint()`.
  - If the API health endpoint is not reachable or does not return parseable JSON, do not block; continue with the existing behavior (let downstream steps fail naturally).

## Constraints

- No runtime API changes.
- Keep behavior safe-by-default.
- Only gate when health JSON provides an explicit DB status.

## Acceptance Criteria

- With API+web running and DB down, running `node ./scripts/verify-live-with-stripe-proxy-auto.mjs --skip-flow` (no `--start-live`) exits with code `2` and prints the DB hint, without invoking `pnpm smoke:live` and without invoking Stripe smoke.
