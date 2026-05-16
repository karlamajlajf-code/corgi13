---
id: continue-20260106-034000-nonstartlive-preflight-status-details
timestamp: 2026-01-06T03:40:00Z
executed: true
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Improve the non-`--start-live` reachability preflight in `tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs` so the failure message is more accurate and actionable by including HTTP status codes (e.g. 404 vs connection refused), and by checking API + web proxy health in parallel to reduce wait time.

## Scope

- Update `tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs` only.
- Only affect the non-`--start-live` preflight behavior.
- Keep exit code semantics: prerequisites missing/unhealthy should remain exit code `2`.

## Required Changes

1. Replace the preflight’s boolean `isHttpOk()` checks with a new helper that returns a structured probe result:

   - `{ ok: boolean, status: number, error?: string }`
   - `status` should be `0` for network failures.
   - Use the existing `SMOKE_HEALTH_TIMEOUT_MS` (abort controller) so behavior stays consistent.

2. Perform the API + proxy probes in parallel (e.g. `Promise.all`) so failures don’t wait sequential timeouts.

3. Update the failure hint to print:

   - Which URL failed
   - The HTTP status code (or the network error message)
   - Guidance to either:
     - start services with `pnpm live`, or
     - rerun the wrapper with `--start-live`

## Constraints

- Do not change `--start-live` orchestration.
- Do not change DB gating behavior.

## Acceptance Criteria

- With nothing listening on 8003/3003, the wrapper exits quickly (code `2`) and prints network failure details.
- If a port is occupied by a wrong process that returns non-2xx (e.g. 404), the wrapper prints the status code instead of implying a connection failure.
- `--print-plan` output remains unchanged.
