---
id: std-20260112-211011-continue-e2e-verification
timestamp: 2026-01-12T21:10:11Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue the repo stabilization work by re-verifying the Playwright E2E suite (beyond Chromium) and ensuring the local dev environment is reliably up before running browser-matrix E2E.

## Scope

1. Verify the local environment is healthy:

   - Web app responds at `http://localhost:3003/`.
   - API health responds at `http://localhost:8003/api/v1/health`.

2. Re-run Playwright E2E on additional browser projects (at minimum Firefox) under deterministic settings (`STABLE_TEST_RUN=1`).
3. If failures are due to servers not running (connection refused), restart the dev servers in the background and re-run the affected E2E job.

## Constraints

- Do not change application code unless a real regression is found; prefer treating connection failures as environment issues.
- Keep repo hygiene clean (no new tracked artifacts).

## Acceptance Criteria

- `pnpm smoke:live` passes (web + api endpoints return HTTP 200).
- Playwright E2E for Firefox completes successfully (tests may still be skipped intentionally).
- No new untracked changes in git (workspace stays clean).
