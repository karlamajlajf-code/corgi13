---
id: std-20260112-211539-keep-working-webkit-and-mobile-e2e
timestamp: 2026-01-12T21:15:39Z
executed: true
originalPrompt: "keep working."
---

# Improved Prompt

## Objective

Continue stabilization by re-verifying the Playwright E2E suite on additional browser projects (WebKit and, if feasible, mobile projects) while ensuring the local dev environment remains healthy.

## Scope

1. Confirm environment health before running E2E:

   - Web responds at `http://localhost:3003/`.
   - API responds at `http://localhost:8003/api/v1/health`.

2. Run deterministic Playwright E2E (`STABLE_TEST_RUN=1`) for:

   - WebKit (`--project=webkit`).
   - Optionally Mobile Chrome/Safari projects if configured.

3. If E2E fails with connection errors (e.g., connection refused), treat it as an environment issue:

   - Restart API + web dev servers in the background (nohup) and re-run the failed E2E job.

## Constraints

- Do not change app code unless a real deterministic regression is found.
- Keep repo hygiene clean (no new tracked artifacts).

## Acceptance Criteria

- `pnpm smoke:live` passes.
- `nx run @tejo/root:e2e -- --project=webkit` completes successfully (tests may be skipped intentionally).
- No uncommitted git changes in `tejospec/`.
