---
id: std-20260109-000500-tejospec-fix-addcardform-hook-deps
timestamp: 2026-01-09T00:05:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Resolve the remaining ESLint warning in `@tejo/web` caused by `react-hooks/exhaustive-deps` in `AddCardForm.tsx`, then re-run the narrowest Nx verification targets to confirm the web app is clean.

## Scope

- Update `tejospec/apps/web/src/components/account/AddCardForm.tsx` to satisfy `react-hooks/exhaustive-deps` without changing runtime behavior.
- Re-run Nx checks for `@tejo/web`:
  - `lint`
  - `typecheck`

## Constraints

- Prefer Nx targets (`pnpm nx run ...`) over running underlying tools directly.
- Avoid changing UI strings/translation keys unless required.

## Acceptance Criteria

- `pnpm nx run @tejo/web:lint` completes with **no warnings or errors**.
- `pnpm nx run @tejo/web:typecheck` completes successfully.
- Code change is minimal and does not alter user-visible behavior.
