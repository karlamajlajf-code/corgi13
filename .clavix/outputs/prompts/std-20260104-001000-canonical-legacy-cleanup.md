---
id: std-20260104-001000-canonical-legacy-cleanup
timestamp: 2026-01-04T00:10:00Z
executed: false
originalPrompt: "Continue Phase 3 wholesale work; align to canonical apps and remove legacy changes"
---

# Improved Prompt

## Objective

Stabilize and complete Phase 3 (B2B wholesale) by ensuring the **canonical** Nx monorepo apps (`tejospec/apps/api` and `tejospec/apps/web`) are the only source-of-truth for B2B wholesale behavior, and that repo guardrails + verification scripts pass.

## Context

This repo contains legacy implementations under `tejospec/frontend/**` and `tejospec/backend/**`. Recent changes added App Router pages and API routes under `tejospec/frontend/web/src/app/**`, but the canonical web app is **Next.js Pages Router** under `tejospec/apps/web/src/pages/**`.

Canonical wholesale features already exist in:

- API: `tejospec/apps/api/src/modules/wholesale/**` and `.../wholesale-catalog/**`
- Web: `tejospec/apps/web/src/pages/auth/signup-b2b.tsx`, `.../checkout/b2b.tsx`, `.../wholesale/*`, and admin pages.

## Scope

1. Remove or revert **legacy folder** code changes that duplicate canonical features (especially anything under `tejospec/frontend/web/src/app/**` and other `tejospec/frontend/**` additions), to satisfy canonical-only guardrails.
2. Ensure the canonical B2B pages and APIs remain intact and consistent with the smoke/verify scripts.
3. Verify the repo with the narrowest relevant checks:
   - strict working-tree guardrails (fail on legacy/gen output)
   - targeted wholesale verification (at least script-level; runtime if stack is available)

## Constraints

- Do not migrate the legacy Next App Router implementation into canonical code.
- Prefer Nx-driven commands and scripts declared in `tejospec/package.json`.
- Keep changes surgical and focused on unblocking canonical verification.

## Acceptance Criteria

- No modified/added files remain under `tejospec/frontend/**`, `tejospec/backend/**`, or other legacy roots unless explicitly required.
- Canonical web routes continue to exist under `tejospec/apps/web/src/pages/**`.
- `pnpm -C tejospec check:working-tree:strict` (or equivalent) passes.
- Wholesale verification scripts can run without failing due to legacy folder changes; if runtime services are unavailable, the repository is still in a state where `pnpm verify:wholesale` is runnable once the stack is up.
