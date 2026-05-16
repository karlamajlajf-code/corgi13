---
id: std-20251222-191500-continue-legacy-entrypoint-banners
timestamp: 2025-12-22T19:15:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Reduce documentation confusion by clearly labeling legacy entrypoint docs in `tejospec/` and directing readers to the canonical runbook.

## Scope

Update these legacy/high-visibility markdown docs to include a short banner at the top:

- `tejospec/TASKS.md`
- `tejospec/QUICK_SUMMARY.md`
- (Optional) `tejospec/PROJECT_STRUCTURE.md` if it could be mistaken as canonical

## Constraints

- Do not rewrite the legacy content; only add a clear banner.
- Do not change historical port values inside legacy sections/snippets.
- The banner must direct users to `tejospec/docs/MASTER_PLAN.md` as the canonical Nx+pnpm workflow (apps in `apps/web` + `apps/api`, ports `3003/8003`).
- Keep edits minimal and avoid printing secrets.

## Acceptance Criteria

- Each targeted legacy doc has a visible banner stating it is historical/legacy and pointing to `docs/MASTER_PLAN.md`.
- `tejospec/docs/MASTER_PLAN.md` Change Log has an entry noting the new legacy-doc banners.
- Markdown lint passes for edited files (no MD022/MD032 issues introduced).
