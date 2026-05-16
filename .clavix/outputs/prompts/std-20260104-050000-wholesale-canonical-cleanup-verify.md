---
id: std-20260104-050000-wholesale-canonical-cleanup-verify
timestamp: 2026-01-04T05:00:00Z
executed: true
originalPrompt: "Continue Phase 3 wholesale: enforce canonical Nx-only, delete legacy duplicate B2B routes/components under tejospec/frontend, pass strict working-tree guardrail, then run wholesale verification."
---

# Improved Prompt

## Objective

Enforce the canonical Nx workspace under `tejospec/` as source of truth by removing duplicate legacy B2B frontend files, ensuring strict working-tree guardrails pass (no changes under legacy roots or generated outputs), and running the canonical wholesale verification (proxy + direct API).

## Scope

In-scope:

- Delete legacy/duplicate Next App Router B2B files under `tejospec/frontend/**`.
- Remove any remaining changes under legacy roots in `tejospec/` (legacy roots: `backend/`, `frontend/`, `archive/`).
- Run the strict working-tree guardrail.
- Run wholesale verification using the repo scripts.

Out-of-scope:

- Adding new B2B functionality or migrating legacy implementations into `apps/api` or `apps/web`.

## Constraints

- Canonical applications are:
  - `tejospec/apps/api` (NestJS)
  - `tejospec/apps/web` (Next.js Pages Router)
- Legacy folders must not contain ongoing changes:
  - `tejospec/backend/**`, `tejospec/frontend/**`, `tejospec/archive/**`

## Acceptance Criteria

- The legacy duplicate B2B files under `tejospec/frontend/**` are deleted and not discoverable in the workspace.
- `pnpm check:working-tree:strict` passes from `tejospec/`.
- `pnpm verify:wholesale` passes from `tejospec/`.

## Implementation Steps (run from `tejospec/`)

1. Confirm the legacy duplicate files are absent.
2. Inspect working tree; remove changes under legacy roots if present.
   - Recommended cleanup for legacy backend drift:
     - `git restore backend`
     - `git clean -fd backend`
3. Run strict guardrail:
   - `pnpm check:working-tree:strict`
4. Run wholesale verification:
   - `pnpm verify:wholesale`

## Execution Notes (observed outcome)

- Strict guardrail: PASS
- Wholesale verification: PASS (proxy + api)
