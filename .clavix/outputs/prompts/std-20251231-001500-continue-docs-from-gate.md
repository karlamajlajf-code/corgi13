---
id: std-20251231-001500-continue-docs-from-gate
timestamp: 2025-12-31T00:15:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue stabilizing the canonical wholesale workflow by updating docs to match the tightened B2B gate regression checks (redirect includes both `blocked=b2b` and `from=<pathname>`).

## Scope

- Update `tejospec/docs/WHOLESALE_CATALOG_REQUEST_SMOKE_TEST.md`:
  - In the B2B gate section, state the redirect must include `blocked=b2b` **and** `from=<original pathname>`.
  - Keep `SMOKE_B2B_GATE_REQUIRED=1` guidance intact.
- Update `tejospec/docs/LEGACY_WHOLESALE_TO_CANONICAL_MAPPING.md`:
  - Mention the redirect includes `from=<pathname>`.

## Acceptance Criteria

- Docs accurately describe runtime behavior.
- `pnpm verify:wholesale` passes.
