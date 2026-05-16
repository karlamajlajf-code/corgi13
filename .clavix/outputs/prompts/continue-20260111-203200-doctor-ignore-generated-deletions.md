---
id: continue-20260111-203200-doctor-ignore-generated-deletions
timestamp: 2026-01-11T20:32:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Reduce `doctor --strict` noise by making the working-tree drift check ignore deletions under generated output roots (e.g. staged deletions of `playwright-report/*` and `test-results/*`).

## Scope

- Update `tejospec/scripts/doctor.mjs` working-tree drift logic to:
  - Parse porcelain status codes.
  - Exclude deletion-only changes under generated roots from the generated drift count.
- Keep legacy detection unchanged.

## Constraints

- Do not weaken detection of changes in legacy roots.
- Keep `doctor --strict` behavior the same for real warnings/failures.

## Acceptance Criteria

- With staged deletions under generated roots, `pnpm tejo:doctor:strict` shows `generated=0` (or otherwise does not count those deletions) and exits 0.
