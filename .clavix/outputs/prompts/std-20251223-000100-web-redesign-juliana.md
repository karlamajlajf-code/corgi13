---
id: std-20251223-000100-web-redesign-juliana
timestamp: 2025-12-23T00:01:00Z
executed: false
originalPrompt: "what happened with previous style and design??? i like this, but it must be like https://juliana-nails.com/"
---

# Improved Prompt

## Objective

Update the Tejo storefront UI (`tejospec/apps/web`) so the **style, layout, and shopping vibe** closely match `https://juliana-nails.com/` (hero promos, top shipping bar, clean ecommerce header, sections like Highlights/Bestseller/Newsletter), while keeping the Tejo codebase original (no copied HTML/CSS/assets from Juliana).

## Scope

- Add a global `Layout` wrapper with:
  - Top info bar (free shipping / delivery time / hotline)
  - Header with logo, primary nav links (e.g. Gel Lack, Sets, Neuheiten, Angebote), search, account, wishlist, cart
  - Footer with trust/links/newsletter
- Homepage structure aligned to Juliana’s feel:
  - Full-width promotional hero banners/slider with strong CTA (e.g. “JETZT SHOPPEN”)
  - “Highlights für dich” section with 3–6 feature tiles
  - “Bestseller” product grid (use existing data hooks or safe placeholders)
  - Newsletter bonus section and secondary promo bands
- Visual design:
  - Prefer a clean ecommerce look (light background + pink accent) unless the repo already has a deliberate dark theme.
  - Add consistent typography (headline font + body font), spacing scale, button styles, and card styles.
  - Ensure mobile-first and matches Juliana’s “big banner + product grid” rhythm.

## Constraints

- Do not copy/paste Juliana’s code or assets.
- Keep existing routes and functionality working.
- Keep i18n intact (`next-i18next`).
- Ensure API proxy defaults to the local API port used by this repo.

## Acceptance Criteria

- `pnpm --filter @tejo/web dev` serves `http://localhost:3003` without errors.
- Homepage has: top bar + header + hero promos + highlights + bestseller + newsletter + footer.
- Styling looks strongly inspired by Juliana’s ecommerce presentation (banner-heavy, clean cards, prominent CTAs) while being original.
- `pnpm typecheck` and `pnpm test` pass for the workspace.
