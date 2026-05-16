---
id: continue-20260104-010-phase4-next-track
timestamp: 2026-01-04T00:00:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue Phase 4 work in the canonical Nx workspace under `tejospec/`, keeping `@tejo/api` and `@tejo/web` checks green.

## Current State

- Phase 4 hardening for admin-only endpoints is complete and validated (roles decorator + guard; controllers migrated).
- `tasks.md` has Phase 4 feature tracks still pending:
  - Loyalty Program (models + API + dashboard)
  - Blog & Content Hub (CMS + pages + SEO)
  - Search & Recommendations (API + UI integration)
  - Subscriptions (billing + management portal)

## Clarifying Question (required to proceed)

Which Phase 4 track should be implemented next?

Options:

- A) Loyalty Program
- B) Blog & Content Hub
- C) Search & Recommendations
- D) Subscriptions

## Constraints

- Work only in canonical code: `tejospec/apps/api/**`, `tejospec/apps/web/**`, and shared packages as needed.
- Avoid changes under legacy folders (`tejospec/backend/**`, `tejospec/frontend/**`, `tejospec/archive/**`).
- Prefer small, testable increments.

## Acceptance Criteria

- Selected feature track has an initial, end-to-end slice implemented (API + web where relevant) without breaking existing behavior.
- Verification passes for the impacted targets (at minimum `pnpm exec nx run @tejo/api:lint` and/or `pnpm exec nx run @tejo/web:lint`, plus build/test where relevant).
