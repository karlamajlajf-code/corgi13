---
id: continue-20260110-055000-reduce-e2e-console-noise
timestamp: 2026-01-10T05:50:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Reduce recurring console noise during E2E test runs to improve debugging signal quality, specifically targeting:

- Benign 401 errors from `/api/auth/me` calls in unauthenticated flows
- Next.js Image "missing sizes" warnings
- Any other high-frequency console warnings that don't indicate real issues

## Scope

- Identify the most frequent console warnings by reviewing recent E2E output
- For benign 401s: either stub the endpoint in unauthenticated flows or suppress expected console errors
- For Next Image warnings: add proper `sizes` attributes where missing or configure Image component defaults
- Keep changes minimal and test-specific where possible (avoid production code changes unless clearly beneficial)

## Constraints

- Do not suppress legitimate errors or warnings
- Maintain current E2E stability (152 passing tests)
- Changes should not affect production behavior
- Verify via targeted E2E run after changes

## Acceptance Criteria

- Console output during E2E runs shows significantly reduced noise (at least 50% fewer warnings)
- All existing passing tests remain green
- No new console errors introduced
- Changes are documented inline with comments explaining why warnings were addressed
