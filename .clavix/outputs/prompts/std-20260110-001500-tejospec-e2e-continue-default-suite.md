---
id: std-20260110-001500-tejospec-e2e-continue-default-suite
timestamp: 2026-01-10T00:15:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Keep stabilizing the Playwright E2E suite for the **apps/web** storefront running at `http://localhost:3003` by running the default suite (chromium first), identifying the next failing non-gated spec, and fixing it in a locale-agnostic way.

## Scope

- Run the Playwright test suite (start with `--project=chromium`) with default environment (do not enable `E2E_ADMIN_UI` or `E2E_LEGACY_UI`).
- Fix only the specs/helpers needed to make the default suite green against `baseURL=http://localhost:3003`.
- Prefer stable selectors (`data-testid`) and deterministic state resets (`localStorage` for persisted stores).

## Constraints

- Avoid translated text assertions (Croatian-first UI).
- Do not “fix” legacy/immersive specs unless they are supposed to run by default (they should remain opt-in).

## Acceptance Criteria

- `STABLE_TEST_RUN=1 pnpm exec playwright test --project=chromium` completes with no failures (gated suites should skip).
- Any fixes include rerunning the narrowest failing spec(s) first, then re-running the chromium suite.
