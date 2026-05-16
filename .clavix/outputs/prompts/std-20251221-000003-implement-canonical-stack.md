---
id: std-20251221-000003-implement-canonical-stack
timestamp: 2025-12-21T00:00:03Z
executed: true
originalPrompt: "Start implementation"
---

# Improved Prompt

## Objective

Implement a canonical monorepo architecture under `tejospec/`:

- **Package manager**: `pnpm` only (no npm-based installs in CI/scripts).
- **Website**: Next.js **Pages Router** app, with **Turbopack** for dev.
- **API**: NestJS app following module architecture.
- **Shared**: a single shared package imported by web+api; **shared must never import from web or api**.
- Maintain a single living plan doc with checkbox tasks and mark completed items.

## Scope

- Create/update `tejospec/docs/MASTER_PLAN.md` (single source of truth) and keep it updated.
- Canonicalize workspace membership via `tejospec/pnpm-workspace.yaml` (prefer `apps/*` + `packages/*`).
- Update `tejospec/apps/web` to enable Turbopack dev command while staying Pages Router.
- Add enforcement for one-way shared dependencies using:
  1. ESLint `no-restricted-imports`
  2. Nx module boundaries (`@nx/enforce-module-boundaries`)
- Pin Nx tooling in `tejospec/package.json` and update CI workflow(s) to use `pnpm exec nx`.

## Constraints

- Do not print contents of any `.env*` files.
- Prefer minimal, safe edits; preserve runtime behavior.

## Acceptance Criteria

- `tejospec/docs/MASTER_PLAN.md` exists and tracks tasks with `- [ ]` / `- [x]`.
- `tejospec/apps/web/package.json` dev uses Turbopack and remains Pages Router.
- Shared import boundary is enforced by ESLint + Nx rules.
- CI workflow uses `pnpm` and a pinned `nx` (no `npx nx` downloading unknown versions).
