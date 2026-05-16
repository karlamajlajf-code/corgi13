---
id: std-20251229-000300-continue-seed-any-cleanup
timestamp: 2025-12-29T00:03:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Reduce noisy lint output by removing the remaining `@typescript-eslint/no-explicit-any` warnings in the Prisma seed script.

## Context

Husky/lint-staged now passes, but `apps/api/prisma/seed.ts` still emits a few `no-explicit-any` warnings in lint-staged output.

## Scope

- Replace the remaining `any` annotations/usages in `apps/api/prisma/seed.ts` with safer types (`unknown` or Prisma types).
- Keep behavior identical.

## Acceptance Criteria

- Running `pnpm exec eslint apps/api/prisma/seed.ts` shows 0 `no-explicit-any` warnings.
- `pnpm exec lint-staged` still passes.
