---
id: std-20251223-121500-premium-ui-polish
timestamp: 2025-12-23T12:15:00Z
executed: true
originalPrompt: "make it more smooth and nice, more user attractive"
---

# Improved Prompt

## Objective

Make the `tejospec/apps/web` storefront feel noticeably more premium, smooth, and “luxury minimal” (Juliana-inspired rhythm, not copied), by adding subtle micro-interactions and visual polish to the most visible UI elements.

## Scope

- Apply existing global micro-interaction utilities (`interactive-lift`, `interactive-underline`) to:
  - Primary hero CTA and hero side promo tiles
  - Desktop nav category links + quick links
  - Key header icon links/buttons (account, wishlist, cart, phone)
- Add a subtle “luxury grain/gradient” background (`bg-luxe-grain`) to the hero section or other large dark surfaces where appropriate.
- Keep accessibility intact (focus-visible, keyboard navigation, reduced-motion).

## Constraints

- Do not copy Juliana’s assets/text; only emulate the vibe and interaction quality.
- Avoid breaking the always-on dev flow and ensure loaders remain functional.
- Keep changes minimal and consistent with existing Tailwind + component patterns.

## Acceptance Criteria

- Interactions feel smoother on hover/focus (links underline elegantly; tiles/buttons subtly lift).
- Keyboard users see clear focus indication.
- Reduced-motion users are not forced into animations.
- `nx run @tejo/web:typecheck` and `nx test @tejo/web` pass.
