---
id: std-20260108-140000-tejospec-i18n-legal-pages
timestamp: 2026-01-08T14:00:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue the Tejospec web i18n sweep by removing all hard-coded German legal-page copy and replacing it with `next-i18next` translations, with **Croatian (hr) as the primary language** and **English (en) as the minimum fallback** (to prevent raw i18n key leakage for other locales).

## Scope

Convert these Next.js pages under `tejospec/apps/web/src/pages/`:

- `imprint.tsx`
- `privacy.tsx`
- `terms.tsx`
- `returns.tsx`

For each page:

- Replace all user-visible strings (head title, breadcrumb labels, headings, paragraphs, buttons, placeholders, labels) with `t(...)` calls from the `common` namespace.
- Keep layout, styling, and existing rendering behavior intact.
- For arrays/constants that currently store German text (e.g., `sections`, `returnSteps`, `returnReasons`), refactor them to store **translation keys** (not translated values) so state comparisons don’t depend on locale text.

## Constraints

- Use `useTranslation('common')` and keep `getStaticProps` using `getCommonTranslations(locale)`.
- Avoid introducing new namespaces; add new keys under `common.json`.
- Do not allow any raw i18n keys to appear in UI: ensure **all new keys exist in at least**:

  - `tejospec/apps/web/public/locales/hr/common.json`
  - `tejospec/apps/web/public/locales/en/common.json`

## Acceptance Criteria

- No hard-coded German strings remain in the four target pages (except non-translatable identifiers like emails, URLs, phone numbers, company names, VAT IDs).
- `hr/common.json` and `en/common.json` contain complete translations for all new keys used by those pages.
- Pages compile and typecheck.

## Verification

Run the narrowest checks for the web app:

- Prefer Nx targets if available; otherwise run the package scripts:
  - `lint` and `typecheck` for `@tejo/web`.
