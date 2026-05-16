---
id: std-20260104-020000-continue-canonical-cleanup
timestamp: 2026-01-04T02:00:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue Phase 3 wholesale stabilization by removing newly-added _legacy_ Next App Router B2B/wholesale duplicates under `tejospec/frontend/**`, keeping the Nx canonical apps (`tejospec/apps/api`, `tejospec/apps/web`) as the single source of truth, then verify repo hygiene and provide a runnable verification path.

## Scope

1. **Do not change canonical behavior** in `tejospec/apps/api/**` or `tejospec/apps/web/**` unless a direct break is discovered.
2. **Remove duplicate legacy B2B implementation** added under `tejospec/frontend/web/src/app/**` and `tejospec/frontend/web/src/components/b2b/**`:
   - `tejospec/frontend/web/src/app/auth/signup-b2b/page.tsx`
   - `tejospec/frontend/web/src/app/checkout/b2b/page.tsx`
   - `tejospec/frontend/web/src/app/admin/wholesale-approvals/page.tsx`
   - `tejospec/frontend/web/src/app/api/orders/b2b/route.ts`
   - `tejospec/frontend/web/src/components/b2b/BulkOrderUpload.tsx`
3. Keep non-legacy additions under `tejospec/scripts/**` and `tejospec/docs/growth/**` intact (unless they directly block verification).

## Constraints

- Treat `tejospec/` as the git root (contains `.git/`).
- Prefer minimal, surgical deletions: remove only the newly added duplicate legacy files.
- After deletions, enforce canonical-only development by running `pnpm -C tejospec check:working-tree:strict`.

## Verification

- Repo hygiene: `pnpm -C tejospec check:working-tree:strict` must pass (no changes under legacy roots `backend/`, `frontend/`, `archive/`).
- If stack is running, run wholesale verification:
  - `pnpm -C tejospec verify:wholesale` (proxy + direct API modes)

## Deliverable

- Legacy duplicate B2B files removed.
- Working-tree check passes.
- Clear next commands for the user to run the wholesale smoke/verify scripts.
