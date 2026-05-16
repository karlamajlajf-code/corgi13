---
id: std-20260103-171200-continue-verify-live-post-reverify
timestamp: 2026-01-03T17:12:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Run the canonical live runtime verification after recent formatting edits and after all static gates are green.

## Scope

- Canonical workspace only: `tejospec/`
- Execute the live smoke verification:
  - `pnpm -s verify:live`

## Constraints

- No code changes.

## Acceptance Criteria

- `pnpm -s verify:live` passes (API + web endpoints + basic flow smoke).
