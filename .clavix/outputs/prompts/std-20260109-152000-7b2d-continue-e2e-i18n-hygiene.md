---
id: std-20260109-152000-7b2d
name: continue-e2e-i18n-hygiene
depthUsed: standard
timestamp: 2026-01-09T15:20:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue repo stabilization by removing German coupling from Playwright E2E and fixing known i18n data issues, while keeping Croatian (hr) as the primary UX language.

## Scope (Implement Now)

1. **Fix Playwright E2E selector drift** so login flows work reliably.
2. **Remove German text coupling from E2E tests** by replacing brittle text assertions with language-agnostic selectors (preferred), or Croatian text where unavoidable.
3. **Deduplicate locale JSON keys** that currently silently override each other.

## Target Files

- E2E specs:
  - `tejospec/tests/e2e/specs/account-pages.spec.ts`
  - `tejospec/tests/e2e/specs/auth-flows.spec.ts`
- Web pages (reference only unless changes needed):
  - `tejospec/apps/web/src/pages/auth/login.tsx`
- Locale dictionaries:
  - `tejospec/apps/web/public/locales/hr/common.json`
  - `tejospec/apps/web/public/locales/en/common.json`

## Requirements

### A) E2E selector fixes

- Update login helper selectors to match actual DOM (e.g., `#email`, `#password`) rather than `name=` attributes.
- Prefer Playwright locators (`page.getByRole`, `page.locator`) over raw CSS where practical.

### B) E2E assertion strategy

- Eliminate German literal strings in assertions/click targets.
- Prefer **stable selectors** (role, label, id, data-testid if already present) over visible text.
- Keep tests functionally meaningful: verify routing + key form controls exist + key state transitions.

### C) Locale JSON dedupe

- Remove duplicated top-level keys:
  - `rewardsPage` appears twice in HR and EN.
  - `passwordMismatch` appears twice in EN.
- Keep the schema actually used by code (e.g., `rewardsPage.head.title` used by `rewards.tsx`).
- After edits, ensure JSON parses successfully.

## Verification

- Parse the modified JSON files via a Node one-liner (must succeed).
- Prefer running checks via Nx (where feasible) for any E2E or lint targets.

## Acceptance Criteria

- No remaining German UI text literals in the two targeted E2E spec files.
- E2E login helper uses selectors that exist in `login.tsx`.
- `hr/common.json` and `en/common.json` contain only one `rewardsPage` key each; `en/common.json` contains only one `passwordMismatch` key.
- JSON parsing verification passes.

## Quality Scores

- **Clarity**: 80%
- **Efficiency**: 70%
- **Structure**: 85%
- **Completeness**: 80%
- **Actionability**: 85%
- **Specificity**: 80%
- **Overall**: 81% (good)

## Original Prompt

```text
continue
```
