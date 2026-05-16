---
id: continue-20260106-181500-localhost-fallback-probes
timestamp: 2026-01-06T18:15:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Improve reliability of `tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs` on Windows by eliminating false-negative preflight failures caused by `localhost` IPv4/IPv6 binding mismatches.

## Context

The wrapper’s non-`--start-live` reachability preflight uses `fetch()` against:

- `http://localhost:8003/api/v1/health`
- `http://localhost:3003/api/health`

In the recent mock-harness run, both mock servers were bound to `127.0.0.1`, but `fetch('http://localhost:...')` failed (likely `localhost -> ::1`), causing the wrapper to exit early with "fetch failed" before it could validate the Stripe-route probe.

## Required Changes

Update `tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs` so that all internal HTTP probes (`probeHttpStatus`, `isHttpOk`, `readHealthJsonOrNull`, and Stripe-route probing) automatically retry `localhost` URLs using:

1. `127.0.0.1`
2. `[::1]`

…when the original `fetch()` throws a network error.

## Constraints

- Preserve existing semantics:
  - Health preflight still requires 2xx for API health and proxy health.
  - Stripe-route early failure still triggers only on `404` (401/403 are acceptable “route exists” signals).
  - Exit code 2 remains reserved for missing prerequisites.
- Keep the change scoped to this wrapper script only.

## Acceptance Criteria

- Running the wrapper against mock servers bound to `127.0.0.1` succeeds in reachability preflight (no more `fetch failed`) and reaches the Stripe-route probe.
- Stripe-route probe deterministically fails early with the existing 404 message/exit code when Stripe routes are missing.
- `node ./scripts/verify-live-with-stripe-proxy-auto.mjs --print-plan` still works and prints an unchanged plan.
