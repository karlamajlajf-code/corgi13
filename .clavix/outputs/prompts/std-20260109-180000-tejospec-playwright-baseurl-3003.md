---
id: std-20260109-180000-tejospec-playwright-baseurl-3003
timestamp: 2026-01-09T18:00:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Unblock and stabilize Playwright E2E runs by aligning the default Playwright `baseURL` with the repo’s actual frontend port mapping (Docker + dev scripts), and re-run a narrow E2E verification for the two recently-edited specs.

## Context

- `apps/web` runs on port **3003** (`next dev -p 3003`, `next start -p 3003`).
- `docker-compose.yml` maps the frontend as `3003:3003`.
- The current `playwright.config.ts` defaults to `http://localhost:3000`, which causes `net::ERR_CONNECTION_REFUSED` when the stack is running on 3003.

## Scope

1. Update `tejospec/playwright.config.ts`:
   - Change `use.baseURL` default from `http://localhost:3000` to `http://localhost:3003`.
   - Keep `PLAYWRIGHT_BASE_URL` override behavior unchanged.
2. Verify locally:
   - Confirm `curl http://localhost:3003` succeeds (server reachable).
   - Run Playwright for **only**:
     - `tests/e2e/specs/auth-flows.spec.ts`
     - `tests/e2e/specs/account-pages.spec.ts`
     - (Chromium only, 1 worker)

## Constraints

- Do not reintroduce any German-coupled assertions.
- Keep changes minimal and isolated to Playwright configuration.

## Acceptance Criteria

- Running the above narrow Playwright command no longer fails immediately with `ERR_CONNECTION_REFUSED` due to incorrect baseURL.
- Playwright config still supports overriding `PLAYWRIGHT_BASE_URL`.

## Verification Steps

- `pnpm -s nx run @tejo/web:lint` (quick sanity)
- `pnpm exec playwright test tests/e2e/specs/auth-flows.spec.ts tests/e2e/specs/account-pages.spec.ts --project=chromium` (with `PLAYWRIGHT_WORKERS=1`)
