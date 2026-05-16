---
id: std-20251222-181500-continue-readme-canonicalize
timestamp: 2025-12-22T18:15:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Reduce onboarding confusion by rewriting the tejospec README to reflect the canonical Nx + pnpm workspace (apps/web + apps/api + packages/shared), the standardized dev ports (web `3003`, api `8003` with `/api/v1`), and to clearly route readers to `docs/MASTER_PLAN.md` as the single source of truth.

## Scope

- Update `tejospec/README.md`:
  - Replace legacy backend/frontend setup instructions with canonical pnpm scripts:
    - `pnpm install`
    - `pnpm env:bootstrap`
    - `pnpm db:bootstrap:flow` (or `pnpm db:up`)
    - `pnpm live`
  - Document canonical URLs:
    - Web: `http://localhost:3003`
    - API: `http://localhost:8003` (health: `http://localhost:8003/api/v1/health`, docs: `http://localhost:8003/api/docs`)
  - Add a prominent pointer to `tejospec/docs/MASTER_PLAN.md`.
  - Add a short “Legacy/Archived” note for `backend/`, `frontend/`, and other non-canonical folders.

## Constraints

- Keep changes minimal and documentation-only (no architectural changes).
- Do not claim the legacy docker-compose/paths are canonical.
- Preserve the repo’s canonical constraints (pnpm-only; Nx preferred; Next.js Pages Router + Turbopack; Nest module architecture).

## Acceptance Criteria

- A new contributor following `tejospec/README.md` lands on the canonical workflow and ports.
- The README explicitly points to `tejospec/docs/MASTER_PLAN.md` for the up-to-date runbook.
- No stale “web runs on 3000” guidance remains in the canonical README.
