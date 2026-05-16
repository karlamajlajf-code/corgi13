---
id: std-20260108-130800-tejospec-i18n-orders-repair-giftcards-keys
timestamp: 2026-01-08T13:08:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue the tejospec i18n sweep with **Croatian as the primary language**, ensuring **no user-visible hard-coded DE/EN strings** remain on key pages and **no raw i18n keys leak** at runtime.

## Scope

1. **Repair build-blocker:** Replace the corrupted `tejospec/apps/web/src/pages/orders.tsx` with a clean, compiling page that:
   - Uses `next-i18next` (`useTranslation('common')`)
   - Uses `getCommonTranslations(locale)` in `getStaticProps`
   - Uses locale-aware date/time formatting
   - Has no German/English hard-coded UI strings
2. **Prevent key leakage:** Add all missing translation keys referenced by:
   - `tejospec/apps/web/src/pages/gift-cards.tsx` (`giftCardsPage.*`)
   - `tejospec/apps/web/src/pages/orders.tsx` (`ordersPage.*`)
     to:
   - `tejospec/apps/web/public/locales/hr/common.json`
   - `tejospec/apps/web/public/locales/en/common.json`
3. **Next i18n cleanup:** Convert any remaining hard-coded demo review content on `tejospec/apps/web/src/pages/product/[slug].tsx` into translation keys and add those keys to `hr/en`.

## Constraints

- Keep Croatian (`hr`) as the default/primary locale.
- Avoid regressions: do not break existing page layouts/styling.
- Follow repo guideline: run checks via **Nx**.

## Acceptance Criteria

- `orders.tsx` is valid TSX and builds.
- `gift-cards.tsx` and `orders.tsx` have **all referenced keys present** in `hr/en common.json`.
- No German strings remain on the updated pages.
- Verification passes (run the narrowest relevant Nx checks first).

## Verification

- Run `nx` checks for the web app (lint + typecheck; add targeted tests only if they exist for these pages).
