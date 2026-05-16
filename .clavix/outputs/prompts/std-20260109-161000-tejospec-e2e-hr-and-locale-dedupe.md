---
id: std-20260109-161000-tejospec-e2e-hr-and-locale-dedupe
timestamp: 2026-01-09T16:10:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Stabilize `tejospec` Playwright E2E tests and locale dictionaries to enforce Croatian-first UX and remove German-coupled assertions.

## Scope

### 1) Fix Playwright login helper selector drift

- Update `tejospec/tests/e2e/specs/account-pages.spec.ts` login helper to fill the actual login form fields (`#email`, `#password`) instead of non-existent `name=` selectors.

### 2) Remove German coupling in E2E specs

- Update Playwright specs to **not** assert/click German UI text.
- Prefer:
  - stable selectors (ids, hrefs, types), and/or
  - Croatian UI strings from `public/locales/hr/common.json` when text assertions are needed.

Target files:

- `tejospec/tests/e2e/specs/account-pages.spec.ts`
- `tejospec/tests/e2e/specs/auth-flows.spec.ts`

### 3) Deduplicate locale JSON keys (prevent silent overrides)

- Remove the duplicate top-level `rewardsPage` block in both:
  - `tejospec/apps/web/public/locales/hr/common.json`
  - `tejospec/apps/web/public/locales/en/common.json`
- Keep the **later** `rewardsPage` schema that matches `tejospec/apps/web/src/pages/rewards.tsx` (uses nested keys like `rewardsPage.head.title`, `rewardsPage.hero.*`, etc.).

## Constraints

- Do not change product UX behavior.
- Keep tests stable and Croatian-first; do not reintroduce any German assertions.
- Keep JSON valid (no duplicate keys at the same level).

## Acceptance Criteria

- `loginAsAdmin()` fills `#email` and `#password` and submits successfully.
- E2E specs no longer contain German literals (e.g., `Bearbeiten`, `Speichern`, `Passwort vergessen`, etc.).
- `hr/common.json` and `en/common.json` each contain only one top-level `rewardsPage` object (the schema used by `rewards.tsx`).
- Fast verification:
  - `node -e "JSON.parse(fs.readFileSync(...))"` passes for both locale files.
  - `pnpm -C tejospec exec playwright test --list` succeeds for the updated specs.
