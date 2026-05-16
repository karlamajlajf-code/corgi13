---
id: std-20251229-235000-continue-wholesale-route-alignment
timestamp: 2025-12-29T23:50:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Finish the canonical wholesale flow hardening by:

1. Aligning canonical NestJS wholesale routes with the global `/api/v1` prefix and the Next.js `/api/*` proxy contract.
2. Producing clear, non-destructive decision support for what to do with legacy Express wholesale code under `backend/**`.

## Scope

- Fix `apps/api` wholesale controller routing so canonical paths resolve to `/api/v1/wholesale/**` (not `/api/v1/api/wholesale/**`).
- Ensure `GET /wholesale/tiers` is publicly accessible as documented, while other wholesale endpoints remain protected.
- Add a mapping/decision doc comparing:
  - legacy Express endpoints in `backend/src/api/routes/**`
  - canonical Nest modules in `apps/api/src/modules/**`
  - canonical web pages that call wholesale endpoints in `apps/web/src/pages/**`
- Update triage documentation to link to the mapping doc and recommend `pnpm tejo:preflight`.

## Constraints

- Do not delete or migrate legacy backend code without explicit user approval.
- Keep `pnpm verify:wholesale` passing.
- Keep changes minimal and canonical-first (Nx apps are the source of truth).

## Acceptance Criteria

- Canonical routes for `apps/api` wholesale controller resolve under `/api/v1/wholesale/**`.
- `pnpm verify:wholesale` passes.
- A doc exists that makes it clear whether the canonical web B2B pages are currently backed by canonical API endpoints, and what the migration/cleanup options are.
