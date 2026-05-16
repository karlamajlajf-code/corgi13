---
id: std-20251222-193500-continue-more-legacy-doc-banners
timestamp: 2025-12-22T19:35:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Reduce confusion from legacy documentation by labeling additional high-visibility entrypoint docs in `tejospec/` as historical/legacy and directing contributors to the canonical runbook.

## Scope

- Add a short legacy banner to these docs:
  - `tejospec/DEPLOYMENT_CHECKLIST.md`
  - `tejospec/EXECUTION_STATUS.md`
  - `tejospec/PROJECT_ANALYSIS_REDESIGN.md`
- Keep legacy content intact (commands, ports, and historical notes remain unchanged).
- If these docs contain large amounts of pre-existing markdownlint violations, add a minimal `markdownlint-disable` comment near the top (do not perform large reformatting).

## Constraints

- Canonical workflow remains in `tejospec/docs/MASTER_PLAN.md` (apps in `apps/web` + `apps/api`, ports `3003/8003`, pnpm-only).
- Do not modify legacy port values in historical sections/snippets.
- Avoid printing secrets.

## Acceptance Criteria

- Each targeted doc has a clear legacy banner directing readers to `docs/MASTER_PLAN.md`.
- `tejospec/docs/MASTER_PLAN.md` Change Log records the addition of these new legacy doc banners.
- `get_errors` reports no markdownlint errors for the edited docs.
