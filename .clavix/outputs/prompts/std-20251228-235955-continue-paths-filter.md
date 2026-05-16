---
id: std-20251228-235955-continue-paths-filter
timestamp: 2025-12-28T23:59:55Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Reduce CI noise by making the wholesale smoke GitHub Actions workflow run only when relevant files change.

## Scope

- Update `.github/workflows/smoke-wholesale.yml` to add `paths` filters for `push` and `pull_request`.
- Include at least:
  - `tejospec/**`
  - `.github/workflows/smoke-wholesale.yml`

## Constraints

- Preserve existing triggers/branches.
- Keep YAML valid and consistent with repo formatting.

## Acceptance Criteria

- Workflow triggers only when matching paths change.
- Workflow still supports running on PRs to main/master for relevant changes.
