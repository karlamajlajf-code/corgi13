---
id: std-20251230-005200-continue-doc-b2b-gate-check
timestamp: 2025-12-30T00:52:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue stabilizing the canonical Tejospec wholesale workflow by updating documentation to reflect the new B2B gating regression checks that run as part of the wholesale smoke/verify scripts.

## Scope

- Update `tejospec/docs/WHOLESALE_CATALOG_REQUEST_SMOKE_TEST.md` to document:
  - The default-off unfinished B2B pages and the env flag `NEXT_PUBLIC_ENABLE_B2B_WHOLESALE`.
  - That `pnpm verify:wholesale` (proxy mode) now checks those B2B pages redirect to `/wholesale/catalog-request?blocked=b2b`.
  - How to enforce gating strictly via `SMOKE_B2B_GATE_REQUIRED=1` (used in CI wrappers).

## Acceptance Criteria

- Docs accurately describe the current behavior.
- `pnpm verify:wholesale` remains green.
