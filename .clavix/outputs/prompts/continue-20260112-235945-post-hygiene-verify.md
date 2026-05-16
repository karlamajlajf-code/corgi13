---
id: continue-20260112-235945-post-hygiene-verify
# Note: timestamp is UTC
timestamp: 2026-01-12T23:59:45Z
executed: true
originalPrompt: "continue!"
---

# Improved Prompt

## Objective

Continue autonomously by verifying the repo remains default-green after the recent Playwright artifact hygiene commit.

## Scope

### Step 0 — Preflight (detect whether the required local stack is running)

- Check whether the storefront is reachable at `http://localhost:3003`.
- Check whether the API health endpoint is reachable at `http://localhost:8003/api/v1/health`.

### Step 1 — Doctor (only if API is reachable)

Run the strict repo doctor script:

```bash
pnpm -s tejo:doctor:strict
```

If the API is not reachable, skip doctor and report that the stack must be running for this check.

### Step 2 — Playwright E2E smoke (only if storefront is reachable)

Run a single Playwright project (Chromium) through Nx with deterministic workers:

```bash
STABLE_TEST_RUN=1 pnpm -s nx run @tejo/root:e2e --skip-nx-cache -- --project=chromium
```

If the storefront is not reachable, skip E2E and report that the stack must be running (Playwright `webServer` is disabled in `playwright.config.ts`).

### Step 3 — Repo cleanliness

Ensure the repo stays clean (Playwright outputs should be ignored):

```bash
git status --porcelain=v1
```

## Constraints

- Do not introduce new product features.
- Only make changes if a check fails, and keep any fix commit tightly scoped.
- Prefer Nx invocations for tasks.

## Acceptance Criteria

- If the API is reachable: `tejo:doctor:strict` exits 0.
- If the storefront is reachable: the Chromium Playwright project exits 0.
- `git status --porcelain=v1` is empty at the end.

## Execution Notes (Completed)

- Preflight:
  - `http://localhost:3003` reachable ✅
  - `http://localhost:8003/api/v1/health` reachable ✅
- `pnpm -s tejo:doctor:strict` ✅
- `STABLE_TEST_RUN=1 pnpm -s nx run @tejo/root:e2e --skip-nx-cache -- --project=chromium` ✅
  - Result: 38 passed / 24 skipped
- Repo clean after run ✅ (`git status --porcelain=v1` empty)
- Playwright root result files ignored ✅ (`git check-ignore -v test-results.json test-results.xml`)
