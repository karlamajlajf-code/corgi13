---
id: std-20251229-083604-continue-docs-discoverability
timestamp: 2025-12-29T08:36:04Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue making the TejoSpec workspace easier to operate and evaluate by improving discoverability of the wholesale smoke tests and growth docs, without changing product behavior.

## Scope

- Add clear links from the canonical TejoSpec README to:
  - The wholesale catalog request end-to-end smoke test checklist.
  - The growth docs index (competitor playbook, messaging pack, 90-day plan).
- Keep changes doc-only unless a link is wrong or a referenced file is missing.

## Constraints

- Minimal edits; do not refactor code.
- Keep the canonical health contract unchanged:
  - API health: `http://localhost:8003/api/v1/health`
  - Web proxy health: `http://localhost:3003/api/health`

## Acceptance Criteria

- README in `tejospec/` includes a short “Wholesale Verification” section with links to the smoke test doc and the scripts.
- README includes a short “Growth Docs” section linking to the growth assets index.
- No typecheck regressions.
