---
id: std-20260104-000300-continue-remaining-changes-breakdown
timestamp: 2026-01-04T00:03:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue by producing a clear breakdown of the remaining Git working tree changes (staged vs unstaged vs untracked), grouped by path, so the next chunk can be isolated cleanly.

## Scope

- Repo root: `tejospec/`
- Snapshot (read-only):
  - Staged files: `git diff --cached --name-only`
  - Unstaged tracked files: `git diff --name-only`
  - Untracked files: `git ls-files --others --exclude-standard`
- Group each list by top-level directory, and by `apps/<project>`.

## Constraints

- No code changes.
- Do not commit.

## Acceptance Criteria

- Console output provides:
  - counts for staged/unstaged/untracked
  - top-level grouping
  - `apps/api` vs `apps/web` grouping
