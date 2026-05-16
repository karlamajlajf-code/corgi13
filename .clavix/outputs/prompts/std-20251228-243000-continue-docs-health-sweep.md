---
id: std-20251228-243000-continue-docs-health-sweep
timestamp: 2025-12-28T00:30:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Eliminate remaining documentation inconsistencies around backend health endpoints so docs, Docker Compose, CI, and smoke scripts all reference the same canonical URL.

## Scope

- Find any references to `http://localhost:8003/health` or `http://localhost:8003/api/health` in `tejospec/**/*.md`.
- Update backend health references to `http://localhost:8003/api/v1/health`.
- Where a doc intends to test the Next proxy, use `http://localhost:3003/api/health` and clarify it rewrites to `/api/v1/health`.

## Constraints

- Documentation-only changes; do not change runtime behavior.
- Keep edits minimal and local to the matching docs.

## Acceptance Criteria

- No stale backend health URLs remain in docs.
- Updated docs pass repo markdown linting.
