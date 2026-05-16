---
id: continue-20260112-234500-verify-e2e-matrix
# Note: timestamp is UTC
timestamp: 2026-01-12T23:45:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue the stabilization workflow by running a broader Playwright verification (beyond Chromium-only) while keeping the repo clean (no new artifacts committed).

## Current State

- Branch: `001-build-comprehensive-ecommerce`
- Working tree: clean (no modified/staged/untracked files)
- Prior minimal verification already passed: `pnpm -s tejo:doctor:strict` and `pnpm -s test:e2e -- --project=chromium --max-failures=1`

## Scope

### Step 1 — Strict environment checks

Run:

```bash
pnpm -s tejo:doctor:strict
```

### Step 2 — Playwright E2E verification across browsers

Use `STABLE_TEST_RUN=1` to force `workers=1` from `playwright.config.ts`.

Run:

```bash
STABLE_TEST_RUN=1 pnpm -s nx run @tejo/root:e2e -- --project=firefox --max-failures=1 --reporter=line --skip-nx-cache
STABLE_TEST_RUN=1 pnpm -s nx run @tejo/root:e2e -- --project=webkit --max-failures=1 --reporter=line --skip-nx-cache
```

Optional:

```bash
STABLE_TEST_RUN=1 pnpm -s nx run @tejo/root:e2e -- --project="Mobile Chrome" --max-failures=1 --reporter=line --skip-nx-cache
```

### Step 3 — Confirm repo stays clean

Run:

```bash
git status --porcelain=v1
```

## Constraints / Guardrails

- Do not commit any generated Playwright outputs (`test-results*.{json,xml}`, `playwright-report/`).
- If a browser project fails, stop and fix only what is required for determinism (selectors, waits, route stubs), then re-run that project.

## Acceptance Criteria

- `tejo:doctor:strict` exits 0.
- Firefox + WebKit E2E checks exit 0 (at least one run each).
- Repo remains clean after verification.
