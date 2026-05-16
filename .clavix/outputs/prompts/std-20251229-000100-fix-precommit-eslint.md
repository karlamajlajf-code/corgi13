---
id: std-20251229-000100-fix-precommit-eslint
timestamp: 2025-12-29T00:01:00Z
executed: true
originalPrompt: "fix it (husky pre-commit / lint-staged ESLint failures)"
---

# Improved Prompt

## Objective

Unblock commits by making the Husky `pre-commit` (lint-staged) pass consistently.

## Context

A previous `git commit` was blocked by ESLint errors during lint-staged:

- Web: `react-hooks/exhaustive-deps` rule not found (missing `eslint-plugin-react-hooks` / config registration).
- API: unused import (`Prisma` defined but never used) reported in `apps/api/src/modules/loyalty/loyalty.service.ts`.
- Some warnings (e.g., `@typescript-eslint/no-explicit-any` in seed) may be present but should not fail the hook unless `--max-warnings=0` is used.

## Scope

1. Ensure `eslint-plugin-react-hooks` is installed in the workspace root and registered in the flat ESLint config for `apps/web/**`.
2. Verify ESLint succeeds (exit code 0) on the exact files that previously failed.
3. Confirm the unused-import error is resolved (either by removing the unused symbol or using it).

## Constraints

- Make minimal, targeted changes.
- Do not introduce broad TypeScript or dependency upgrades unless required to fix the hook.

## Acceptance Criteria

- Running ESLint on the previously failing file set exits with code 0.
- No occurrences of “Definition for rule 'react-hooks/exhaustive-deps' was not found”.
- No “X is defined but never used” errors for the referenced loyalty service file.
