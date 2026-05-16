---
id: continue-20260112-220000-split-remaining-staged-into-commits
timestamp: 2026-01-12T22:00:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Split the remaining large staged change-set into a sequence of clean, reviewable commits (by feature area) **without losing any work** and keeping repo health checks green.

## Current State (Observed)

- Branch: `001-build-comprehensive-ecommerce`
- Working tree changes: ~140
- Staged changes: ~89
- Staged scope is primarily:
  - `apps/web/**` (majority)
  - `apps/api/**` (auth/blog/loyalty/upload/wholesale)
  - `.gitignore`, `TASKS.md`, `packages/shared/package.json`

## Plan / Scope

1. **Safety checkpoint**

   - Create a non-destructive safety stash (`git stash create` + `git stash store`) before altering staging.

2. **Rebuild staging into commits**

   - `git reset` to clear the index (preserve working tree)
   - Create commits in this approximate order:
     1. `chore(repo): update gitignore/tasks/shared package`
     2. `feat(api): auth/profile + upload` (or split further if too large)
     3. `feat(api): wholesale admin + validations`
     4. `feat(api): loyalty/blog updates`
     5. `feat(web): auth pages`
     6. `feat(web): account pages`
     7. `feat(web): wholesale pages + admin pages`
     8. `feat(web): checkout/cart/wishlist`
     9. `feat(web): homepage components/SEO/marketing`
     10. `chore(i18n): update locale dictionaries`
     11. `chore(assets): add demo images`

3. **Verification**
   - After each 2–3 commits (or at the end), run narrow checks:
     - `node ./scripts/smoke-live.mjs`
     - `node ./scripts/doctor.mjs --strict`

## Constraints

- Do not lose any staged/unstaged/untracked changes.
- Avoid interactive commands; use deterministic staging (path-based `git add` / `git restore --staged`).
- Keep commits focused; avoid mixing API + web + i18n + assets in the same commit.

## Acceptance Criteria

- The original work remains present in the repo (no loss).
- The large staged set is split into multiple meaningful commits with clear messages.
- Sanity scripts remain green: `smoke-live` + `doctor --strict` exit 0.
- Provide a concise report of commits created (SHA + scope).
