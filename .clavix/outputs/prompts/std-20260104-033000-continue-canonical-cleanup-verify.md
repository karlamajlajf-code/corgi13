---
id: std-20260104-033000-continue-canonical-cleanup-verify
timestamp: 2026-01-04T03:30:00Z
executed: false
originalPrompt: "Continue: enforce canonical Nx apps; delete legacy Next app-router B2B duplicates; run strict working-tree + wholesale verification"
---

# Improved Prompt

## Objective

Finish the “canonical-only” cleanup for Phase 3 B2B/wholesale by removing legacy duplicate routes/pages under `tejospec/frontend/**` (Next App Router legacy tree), then run the strict repo hygiene guardrail and wholesale verification so the result is runnable and verifiable.

## Scope

1. **Delete the legacy duplicate files** (do not migrate/port; just remove):

- `tejospec/frontend/web/src/app/auth/signup-b2b/page.tsx`
- `tejospec/frontend/web/src/app/checkout/b2b/page.tsx`
- `tejospec/frontend/web/src/app/admin/wholesale-approvals/page.tsx`
- `tejospec/frontend/web/src/app/api/orders/b2b/route.ts`
- `tejospec/frontend/web/src/components/b2b/BulkOrderUpload.tsx`

1. **Verify the deletes actually applied**:

- Confirm each path no longer exists via workspace search.
- Confirm `git status --porcelain` does not list those files.

1. **Run hygiene guardrail** (from `tejospec/`):

- `node ./scripts/check-working-tree.mjs --fail-on-legacy`
- Expect: no changes under legacy roots `frontend/`, `backend/`, `archive/`.

1. **Run wholesale verification** (from `tejospec/`):

- Prefer the repo’s existing script target (likely `pnpm verify:wholesale`).
- If unavailable, locate the intended command in `tejospec/package.json` and run the canonical equivalent.

## Constraints

- Treat Nx canonical apps as source of truth:

  - Backend: `tejospec/apps/api`
  - Frontend: `tejospec/apps/web`

- Do not add or modify any functionality inside `tejospec/frontend/**`; the goal is to remove overlaps.
- Keep changes focused; do not refactor unrelated areas.

## Acceptance Criteria

- The five legacy duplicate files are deleted and absent from the working tree.
- `node ./scripts/check-working-tree.mjs --fail-on-legacy` passes.
- Wholesale verification runs successfully (proxy mode + direct API mode) or fails with actionable, specific diagnostics.
