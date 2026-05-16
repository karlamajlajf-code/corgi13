---
id: std-20260109-000001-tejospec-i18n-about-rewards-components
timestamp: 2026-01-09T00:00:01Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue the repo-wide i18n migration to ensure **Croatian (hr) is the primary UI language** and **no hard-coded German strings** remain in user-facing UI for the next highest-impact pages/components.

## Scope (files to change)

1. **About page**: `tejospec/apps/web/src/pages/about.tsx`
2. **Rewards page**: `tejospec/apps/web/src/pages/rewards.tsx`
3. **Entrance loader**: `tejospec/apps/web/src/components/ui/EntranceLoader.tsx`
4. **Add card form (Stripe)**: `tejospec/apps/web/src/components/account/AddCardForm.tsx`
5. **Product card**: `tejospec/apps/web/src/components/product/ProductCard.tsx`
6. Translation dictionaries:
   - `tejospec/apps/web/public/locales/hr/common.json`
   - `tejospec/apps/web/public/locales/en/common.json`

## Constraints / Rules

- Use `next-i18next` (`useTranslation('common')`) and `t('...')`.
- Ensure **every new translation key exists in BOTH `hr` and `en`** to prevent raw-key leakage.
- For arrays/lists rendered in UI, store **translation keys** (e.g., `titleKey`, `descriptionKey`) rather than localized strings.
- Keep existing UI structure/styling and behavior intact; only replace user-facing text.
- Prefer SSR/SSG translation preloading via `getCommonTranslations(locale)`.

## Implementation Notes

### About page

- Replace all German/English copy in the page with `aboutPage.*` keys (head title/description, hero chip/title/body, mission/values/timeline/team/testimonial/CTA sections, badges such as free shipping/original/returns).
- Convert data arrays (`values`, `team`, `milestones`, `stats`) to key-driven structures.
- Switch `getStaticProps` to use `getCommonTranslations(locale)` for consistency with other pages.

### Rewards page

- Replace all German strings with `rewardsPage.*` keys (breadcrumbs, hero, CTAs, stat labels, section headings/subtitles, membership tier benefit strings, redeem section helper text, “popular” badges, etc.).
- Convert `tiers`, `earnMethods` to key-driven structures (e.g., `benefitsKeys`, `titleKey`, `descriptionKey`).
- Keep point ranges, multipliers, numbers, and currency values as literals.

### EntranceLoader

- Remove hard-coded German default label (`Lädt…`) and English tagline.
- Use common keys (e.g., `entranceLoader.label`, `entranceLoader.tagline`) with `useTranslation('common')`.

### AddCardForm

- Replace all German UI labels/errors/status text with `payment.addCard.*` keys.
- Preserve Stripe-provided error message (`result.error.message`) when present.

### ProductCard

- Replace all German strings (wishlist aria labels, out-of-stock label, quick view label, add-to-cart aria label, badge text like new/bestseller) with `productCard.*` keys.

## Acceptance Criteria

- `about.tsx`, `rewards.tsx`, `EntranceLoader.tsx`, `AddCardForm.tsx`, and `ProductCard.tsx` contain **no hard-coded German UI strings**.
- New keys are added to `hr/common.json` and `en/common.json` with appropriate translations.
- Pages render via i18n without showing raw keys.
- Verification passes with the smallest relevant checks (Nx-based): at least `nx lint` + `nx typecheck` for the web project (or the closest existing Nx targets).
