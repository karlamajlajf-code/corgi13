---
id: std-20251221-000600-continue-master-plan
timestamp: 2025-12-21T00:06:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue executing the canonical monorepo master plan under `tejospec/`, focusing on completing any remaining unchecked items and keeping CI pnpm-only.

## Scope

- Verify the canonical Next.js app is Pages Router-only (no `app/` or `src/app/`).
- Standardize the canonical Next.js app scripts (dev/build/start/lint/typecheck) and keep Turbopack enabled for dev.
- Verify NestJS API uses feature modules and AppModule wiring.
- Update `tejospec/docs/MASTER_PLAN.md` to mark tasks done and add a changelog entry.

## Constraints

- Prefer Nx to run checks (lint/test/build) where possible.
- Keep pnpm-only (no npm installs/scripts in CI).
- Do not print secret contents.

## Acceptance Criteria

- Master plan checkboxes reflect newly completed items.
- Focused lint checks pass for `@tejo/web`, `@tejo/api`, and `@tejo/shared`.
