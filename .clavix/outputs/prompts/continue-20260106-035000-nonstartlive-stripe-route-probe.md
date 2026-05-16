---
id: continue-20260106-035000-nonstartlive-stripe-route-probe
timestamp: 2026-01-06T03:50:00Z
executed: false
originalPrompt: "continue working!"
---

# Improved Prompt

## Objective

Harden `tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs` in non-`--start-live` mode so it fails fast when the API process on port 8003 is _not_ the expected tejospec Nest API (specifically, when Stripe routes are missing).

## Scope

- Update `tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs` only.
- Only affect the non-`--start-live` path (services already running).
- Keep all existing behavior:
  - Reachability preflight (API + web proxy health)
  - DB status gating via `/api/v1/health` JSON (`services.database !== 'up'` => exit 2)
  - Start-live orchestration + takeover ports behavior

## Required Changes

1. After the non-`--start-live` reachability preflight succeeds (API + proxy health are 2xx), run a Stripe-route probe equivalent to the existing `probeStripeRoutesOrThrow()` used in `--start-live`.

   - If the probe detects a 404 for the Stripe route (derived from `API_HEALTH_URL`), exit early with the existing informative error.
   - This should happen **before** running any pnpm smoke steps.

2. Improve the non-`--start-live` reachability failure hint slightly for 404 responses:

   - If API health or proxy health returns status 404, mention that this usually means something is responding but it’s the wrong service/path, and suggest rerunning with `--start-live` (and optionally `--takeover-ports` if the user explicitly wants to kill listeners).

## Constraints

- Keep exit codes deterministic:
  - Missing/unhealthy prerequisites remain exit code `2`.
  - Missing Stripe routes remains the existing exit code.
- Do not add or change package.json scripts.

## Acceptance Criteria

- In non-`--start-live` mode, if `API_HEALTH_URL` is reachable but Stripe routes are missing (404), the wrapper exits before running `pnpm smoke:live` / `pnpm verify:live`.
- If API/web health returns 404, the hint mentions “wrong service/path” as a likely cause.
- `--print-plan` output remains unchanged.
