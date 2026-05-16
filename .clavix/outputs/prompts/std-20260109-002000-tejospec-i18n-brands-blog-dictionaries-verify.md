---
id: std-20260109-002000-tejospec-i18n-brands-blog-dictionaries-verify
timestamp: 2026-01-09T02:00:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Finish the in-progress i18n migration batch by adding the missing translation keys for the Brands page and Blog Post page, then re-run the narrowest relevant Nx checks to ensure no i18n key leakage or test failures.

## Scope

1. Add translation trees to both locale dictionaries:

   - `tejospec/apps/web/public/locales/hr/common.json`: add `brandsPage.*` and `blogPostPage.*` with Croatian copy.
   - `tejospec/apps/web/public/locales/en/common.json`: add matching `brandsPage.*` and `blogPostPage.*` with English copy.

2. Validate dictionaries parse as JSON.

3. Verify the web app:
   - `nx run web:lint`
   - `nx run web:typecheck`
   - Run focused Jest tests for:
     - `forgot-password`
     - `reset-password`

## Constraints

- Keep HR/EN key parity (every referenced key exists in both files).
- Do not reintroduce hard-coded German strings.
- Prefer Nx targets for verification.

## Acceptance Criteria

- `brandsPage` and `blogPostPage` keys exist in both `hr/common.json` and `en/common.json`.
- Locale JSON files parse successfully.
- `@tejo/web` lint and typecheck pass.
- Focused auth tests pass.
