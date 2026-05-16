---
id: std-20251228-242000-continue-docs-health-alignment
timestamp: 2025-12-28T00:20:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Align documentation health-check URLs with the actual API behavior used by Docker Compose, the wholesale smoke scripts, and CI.

## Scope

- Update tejospec/docs/WHOLESALE_CATALOG_REQUEST_SMOKE_TEST.md to reference the correct health endpoint (`/api/v1/health`) and clarify proxy mode (`/api/health` → rewrite to `/api/v1/health`).
- Update tejospec/docs/DOCKER_STACK.md if it still references `http://localhost:8003/health`.

## Constraints

- Keep changes minimal and documentation-only (no behavior changes).
- Preserve existing structure and commands in the docs.

## Acceptance Criteria

- Docs no longer point to the wrong backend health URL.
- Markdown passes repo linting rules (no MD022/MD032 errors).
