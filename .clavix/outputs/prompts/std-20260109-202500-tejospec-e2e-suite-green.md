---
id: std-20260109-202500-tejospec-e2e-suite-green
project: tejospec
timestamp: 2026-01-09T20:25:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Get the overall Playwright E2E suite for `tejospec/` to a stable green state (or as close as possible), removing any remaining brittle selectors or language-coupled assertions.

## Scope

- Run the Playwright E2E suite (Chromium; single worker) against the already-running web app.
- Fix failing tests by:
  - removing strict-mode locator ambiguity (role/level scoping, container scoping)
  - replacing brittle class-string assertions with state-driven assertions
  - stubbing API calls when the E2E environment may not have a reliable backend route

## Constraints / Requirements

- Croatian is the primary UX language.
- No German-coupled UI or test assertions.
- Prefer stable selectors (role + level, ids, hrefs, scoped locators) over broad `getByText(/.../)`.
- Keep Playwright configured to run against `http://localhost:3003` by default (already aligned).

## Acceptance Criteria

- `pnpm exec playwright test --project=chromium --workers=1 --reporter=line` completes with **0 failing tests**, or remaining failures are documented with the exact cause and the next concrete fix.

## Verification

- Ensure `http://localhost:3003` is reachable before running.
- Run Playwright suite (Chromium, 1 worker).
- Run `pnpm exec nx run @tejo/web:lint` if any test file changes were made.
