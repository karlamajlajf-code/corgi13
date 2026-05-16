---
id: std-20251223-001800-juliana-luxury-redesign
timestamp: 2025-12-23T00:18:00Z
executed: false
originalPrompt: "page sections etc and navbar etc like juliana; modern luxury but simple; extra responsive; loading screen when entering website"
---

# Improved Prompt

## Objective

Redesign the `tejospec/apps/web` storefront UI to be Juliana-inspired in structure and ecommerce rhythm (not copied), with a luxury + minimal modern aesthetic, excellent responsiveness, and an entrance/loading screen.

## Scope

- Global navigation: top promo bar, sticky header, category navigation (mega menu on desktop), icons (account/wishlist/cart), language/currency affordances if present, and a mobile-first drawer navigation.
- Homepage sections: banner-heavy hero area, highlights/value props, curated category tiles, bestseller grid, new arrivals, newsletter area, trust/payment footer.
- Motion/UX: subtle micro-interactions (hover/focus), reduced-motion support, and consistent spacing/typography.
- Loading screen: full-screen entrance overlay on first visit and a lightweight route-transition loader.

## Constraints

- Do not copy text, imagery, or brand assets from juliana-nails.com; only emulate layout patterns and vibe.
- Keep the implementation within Tailwind + existing component patterns; no heavy new UI frameworks.
- Maintain accessibility: semantic landmarks, focus states, keyboard navigation, and ARIA for menus.
- Keep pages router architecture (Next.js Pages Router) intact.

## Acceptance Criteria

- Navbar feels “luxury minimal”: clean type, spaced layout, sticky behavior, desktop mega menu, mobile drawer menu; works on 360px to ultrawide.
- Homepage reads like an ecommerce landing: hero promos (2–3 tiles), highlights, categories, bestseller grid, newsletter + trust/footer.
- Entrance loading screen appears when entering the website (initial mount), then disappears smoothly; respects `prefers-reduced-motion`.
- Route transitions show a small loading indicator without blocking interaction unnecessarily.
- No broken builds/tests; `pnpm test` still passes.
