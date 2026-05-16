---
id: continue-20260110-140400-full-matrix-e2e-verify
timestamp: 2026-01-10T14:04:00Z
executed: true
originalPrompt: "CONTINUE"
---

# Improved Prompt

## Objective

Continue hardening the Playwright E2E suite by running the broad/default Nx E2E target (all configured Playwright projects), and resolve the first new failure or recurring browser-console noise that appears.

## Scope

1. Confirm the storefront is healthy on `http://localhost:3003`.
2. Run the broad E2E target via Nx (no `--project=` filter).
3. If any failures occur, fix them at the source (selectors, readiness, deterministic stubs, missing `sizes`, etc.).
4. If recurring console noise appears, eliminate root causes (avoid suppressing logs), except when a test intentionally triggers an error path (e.g., invalid login), in which case keep logs scoped to the relevant tests.

## Constraints

- Prefer `data-testid` selectors; avoid locale-coupled strings.
- Keep admin/legacy E2E gated behind env flags so default runs remain green.
- Use Nx (`pnpm nx run ...`) to execute checks.

## Acceptance Criteria

- `pnpm nx run @tejo/root:e2e -- --max-failures=1` completes successfully.
- No new recurring `BROWSER LOG:` lines in successful runs (beyond intentionally negative-test paths).
