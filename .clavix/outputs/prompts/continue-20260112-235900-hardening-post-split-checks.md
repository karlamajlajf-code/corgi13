---
id: continue-20260112-235900-hardening-post-split-checks
# Note: timestamp is UTC
timestamp: 2026-01-12T23:59:00Z
executed: true
originalPrompt: "NASTAVI RADITI NA PROJEKTU! REKAO SAM DA ME NE PITAS STO DA RADIS, VEC DA GA RADIS KAKO HOCES I KAKO JE NAJBOLJE!"
---

# Improved Prompt

## Objective

Continue work autonomously by doing the next best hardening steps after commit-splitting and E2E stabilization:

1. Run the relevant Nx checks for the recent Jest/unit-test changes.
2. Ensure Playwright output artifacts are not accidentally tracked/committed.
3. Leave the repo in a clean, verified state.

## Scope

### Step 1 — Run web unit tests (Nx)

```bash
pnpm -s nx run @tejo/web:test
```

(Optional if fast) also run:

```bash
pnpm -s nx run @tejo/web:lint
```

### Step 2 — Prevent committing Playwright outputs

- Check whether any of these are tracked in git:
  - `test-results.json`
  - `test-results.xml`
  - `playwright-report/`
  - `test-results/`
- If tracked, remove from index and add to `.gitignore`.

### Step 3 — Verify cleanliness

- Confirm `git status --porcelain=v1` is empty after changes.

## Constraints

- Do not introduce new product feature changes.
- Keep commits small and clearly scoped if any repo hygiene changes are required.

## Acceptance Criteria

- `nx run @tejo/web:test` succeeds.
- Playwright artifacts are not tracked and won’t dirty the repo.
- Repo remains clean.

## Execution Notes (Completed)

- `pnpm -s nx run @tejo/web:test` ✅
- `pnpm -s nx run @tejo/web:lint` ✅
- Playwright root artifacts are now ignored and no longer tracked:
  - Added to `tejospec/.gitignore`: `test-results.json`, `test-results.xml`
  - Untracked and committed cleanup: `385bda7 chore(test): stop tracking playwright result outputs`
- Verified ignored:
  - `git check-ignore -v test-results.json test-results.xml` ✅
- Verified clean working tree:
  - `git status --porcelain=v1` ✅
