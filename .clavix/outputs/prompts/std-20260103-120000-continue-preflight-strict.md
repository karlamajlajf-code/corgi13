---
id: std-20260103-120000-continue-preflight-strict
timestamp: 2026-01-03T12:00:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue hardening the canonical Tejospec workspace by running `pnpm tejo:preflight:strict`, resolving any actionable failures it reports (without expanding scope), and re-running until it passes.

## Scope

- Run `pnpm tejo:preflight:strict` from `tejospec/`.
- If it fails, inspect:
  - working tree drift (legacy/generated changes)
  - doctor strict warnings
- Fix only issues introduced by recent work (wholesale gating + verification scripts + docs).

## Constraints

- Do not implement new wholesale B2B endpoints.
- Do not delete legacy folders unless explicitly requested.

## Acceptance Criteria

- `pnpm tejo:preflight:strict` exits 0.
- Canonical checks remain green: `pnpm verify:wholesale:strict`.
