---
id: continue-20260111-210000-commit-hygiene-split-sanity
timestamp: 2026-01-11T21:00:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Safely isolate the **sanity-script fixes** (doctor/check-working-tree + removal of tracked generated Playwright artifacts) into a dedicated git commit **without losing or disturbing** the user’s current large staged/unstaged change-set.

## Current Observations (from repo state)

- Repo has **144 total working tree changes** and **91 staged**.
- The generated-artifact deletions are currently **staged**:
  - `playwright-report/index.html` (D)
  - `test-results/.last-run.json` (D)
- The sanity scripts are currently **modified but UNSTAGED**:
  - `scripts/doctor.mjs` (M)
  - `scripts/check-working-tree.mjs` (M)

## Scope

1. Create a safety recovery point (stash object) **without modifying** the working tree.
2. Temporarily remove unrelated staged changes from the index (stash staged-only), leaving only the sanity-script set to commit.
3. Stage only:
   - `scripts/doctor.mjs`
   - `scripts/check-working-tree.mjs`
   - the deletions of `playwright-report/index.html` and `test-results/.last-run.json`
4. Create a commit like: `chore(sanity): make doctor strict dev-friendly; ignore generated deletions`.
5. Restore the original staged change-set exactly as it was (from staged stash).

## Constraints

- **Do not lose** any staged/unstaged/untracked changes.
- Prefer operations that are reversible and recorded (stash entry).
- Do not run heavyweight test suites; verify only the relevant sanity scripts.

## Acceptance Criteria

- A new commit exists containing **only** the four targeted paths (2 script modifications + 2 tracked deletions).
- After the commit, the user’s previous staged set is restored (still large, but intact).
- `node ./scripts/smoke-live.mjs` and `node ./scripts/doctor.mjs --strict` still exit 0.
- Provide a concise report including:
  - commit SHA
  - stash references created/used
  - what to do if the user wants to undo the split.
