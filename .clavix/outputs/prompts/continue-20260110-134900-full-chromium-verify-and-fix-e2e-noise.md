---
id: continue-20260110-134900-full-chromium-verify-and-fix-e2e-noise
timestamp: 2026-01-10T13:49:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue hardening the Playwright E2E suite by running a broader verification pass (full Chromium project) and fixing any new failures, flakes, or browser-console noise introduced since the last green run.

## Scope

1. **Pre-flight sanity**

   - Ensure the storefront is reachable at `http://localhost:3003` before blaming E2E.

2. **Broader E2E verification**

   - Run the full Chromium E2E project via Nx.
   - Pay special attention to:
     - `BROWSER LOG:` lines (warnings/errors)
     - 401 noise related to `**/api/auth/me`
     - Next.js `next/image` warnings about `<Image fill />` missing `sizes`

3. **Fix-first approach**
   - If failures or noisy logs appear, fix them at the source (selectors, stubs, missing `sizes`, readiness waits), not by suppressing output.

## Constraints

- Prefer `data-testid` selectors; avoid locale-coupled strings.
- Keep admin/legacy E2E gated behind env flags so default runs stay green.
- Use Nx (`pnpm nx run ...`) to execute checks.

## Acceptance Criteria

- `pnpm nx run @tejo/root:e2e -- --project=chromium --max-failures=1` completes successfully.
- No recurring browser console noise from:
  - `Image ... has "fill" but is missing "sizes" prop`
  - `Failed to load resource: the server responded with a status of 401 (Unauthorized)` (auth hydration)

## Notes

- If the web dev server is unhealthy (e.g., “missing required error components”), restart `@tejo/web:dev` and re-validate with `curl` before rerunning E2E.
