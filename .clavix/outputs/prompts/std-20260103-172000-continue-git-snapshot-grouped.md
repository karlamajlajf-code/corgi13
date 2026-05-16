---
id: std-20260103-172000-continue-git-snapshot-grouped
timestamp: 2026-01-03T17:20:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue by producing a compact, path-grouped snapshot of the canonical Git working tree so we can clearly see what’s left to review/stage after all verification gates passed.

## Scope

- Git repo root: `tejospec/`
- Generate grouped summaries from `git status --porcelain`:
  - Total change count
  - Counts by top-level path segment (e.g., `apps/`, `docs/`, etc.)
  - Separate counts for staged/unstaged/untracked where possible

## Constraints

- No code changes.
- Do not stage/commit anything automatically.

## Acceptance Criteria

- A concise console report exists showing totals + directory grouping.
- Prompt is marked executed after the snapshot is captured.
