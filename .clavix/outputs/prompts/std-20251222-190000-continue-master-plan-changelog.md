---
id: std-20251222-190000-continue-master-plan-changelog
timestamp: 2025-12-22T19:00:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Finish the documentation follow-through for the recent canonicalization work in `tejospec/`:

- Confirm `tejospec/README.md` still reflects the canonical pnpm+Nx workflow and canonical ports.
- Append a concise entry to `tejospec/docs/MASTER_PLAN.md` **Change Log** noting the README canonicalization and the legacy-doc banner addition.
- Run a quick verification command to ensure nothing regressed.

## Scope

- Documentation-only updates:
  - `tejospec/docs/MASTER_PLAN.md` (Change Log section)
- No code refactors unless a doc change reveals a clear mismatch that must be corrected to match the canonical runbook.

## Constraints

- Keep canonical constraints intact: pnpm-only, Nx preferred, Next.js Pages Router + Turbopack, NestJS module architecture, one-way imports where shared never imports web/api.
- Canonical local URLs must remain:
  - Web: `http://localhost:3003`
  - API: `http://localhost:8003/api/v1`
- Do not print or log env file contents or secrets.

## Acceptance Criteria

- `tejospec/README.md` is consistent with the canonical workflow (apps/web + apps/api + packages/shared) and points to `docs/MASTER_PLAN.md`.
- `tejospec/docs/MASTER_PLAN.md` has a new Change Log entry documenting:
  - README canonicalization
  - Legacy-doc banner added to the quick reference doc
- A quick sanity check runs successfully (prefer `pnpm -C tejospec tejo:doctor --skip-db --skip-endpoints`).
