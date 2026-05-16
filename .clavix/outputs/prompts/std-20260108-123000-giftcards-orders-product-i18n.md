---
id: std-20260108-123000-giftcards-orders-product-i18n
timestamp: 2026-01-08T12:30:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue the Croatian-first i18n cleanup in `tejospec/apps/web` by removing remaining hard-coded German customer-facing strings and ensuring all UI text is sourced from `next-i18next` (`common` namespace), with Croatian (`hr`) as the primary locale and English (`en`) as the fallback.

## Scope

Update the following pages to be fully i18n-driven:

1. `tejospec/apps/web/src/pages/gift-cards.tsx`

- Replace all visible hard-coded strings (head title, breadcrumbs, hero copy, section titles, labels/placeholders, CTA buttons, benefit cards, gift set badge/reviews text) with `t('...')` keys under `giftCardsPage.*`.
- Convert data arrays (`giftCardDesigns`, `giftSets`) to store translation keys (and stable IDs) rather than German/English strings.

1. `tejospec/apps/web/src/pages/orders.tsx`

- Replace all visible hard-coded strings with keys under `ordersPage.*`.
- Convert `statusSteps` to use translation keys.
- Convert mock tracking result text (status/location text) to translation keys and/or locale-safe formatting.
- Use locale-aware date formatting (`Intl.DateTimeFormat`) for mock dates so Croatian locale renders Croatian date formatting.

1. `tejospec/apps/web/src/pages/product/[slug].tsx`

- Replace the hard-coded German demo review `title`/`text` (and date formatting) by using translation keys under `product.reviews.defaults.*` (or equivalent), keeping the rest of the page unchanged.

## Dictionary Updates

Update:

- `tejospec/apps/web/public/locales/hr/common.json`
- `tejospec/apps/web/public/locales/en/common.json`

Add:

- `giftCardsPage` tree (all keys required by `gift-cards.tsx`)
- `ordersPage` tree (all keys required by `orders.tsx`)
- `product.reviews.defaults` (demo review titles/texts and any formatting strings)

Other locales may rely on existing fallback behavior (en → hr), but no raw i18n key leaks should occur.

## Constraints

- Preserve existing page layouts, styling, and component structure.
- Do not introduce new dependencies.
- Keep SSR/SSG translation preloading via the existing `getCommonTranslations(locale)` helper.
- Ensure React keys are stable when mapping translated arrays.

## Acceptance Criteria

- Gift Cards, Orders Tracking, and Product pages show Croatian UI text under `hr` locale.
- No German hard-coded UI strings remain in the edited pages.
- `hr/common.json` and `en/common.json` remain valid JSON.
- Nx checks pass for the web app:

  - `nx run web:lint`
  - `nx run web:typecheck`

## Verification

After changes, run the narrowest relevant checks (web lint + typecheck) via Nx and fix any failures introduced by the changes.
