---
id: std-20251228-223000-tejo-homepage-wholesale-banner-strip
timestamp: 2025-12-28T22:30:00Z
executed: false
originalPrompt: "Homepage still feels unchanged on localhost:3003; add an unmistakable, always-visible Wholesale/B2B banner on / so the funnel is impossible to miss."
---

# Improved Prompt

## Objective

Make the Wholesale/B2B funnel **impossible to miss on the homepage** (`/`) so a user immediately perceives a difference at `http://localhost:3003/`.

## Scope

- Add a dedicated, always-visible homepage section (banner/strip) that promotes Wholesale/B2B.
- Include clear CTAs linking to:
  - `/wholesale` (landing)
  - `/wholesale/catalog-request` (lead capture)

## Constraints

- Keep styling consistent with existing Tailwind + homepage design system.
- No routing changes; do not alter wholesale business logic.
- Avoid i18n churn; keep copy short and neutral.

## Acceptance Criteria

- The homepage contains an obvious “Wholesale / B2B” section visible without hunting inside a hero slide.
- The section includes working links to `/wholesale` and `/wholesale/catalog-request`.
- `curl http://localhost:3003/` contains a stable marker string for this section (for automated smoke checks).
