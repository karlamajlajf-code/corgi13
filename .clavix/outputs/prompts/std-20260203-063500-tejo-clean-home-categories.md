---
id: std-20260203-063500-tejo-clean-home-categories
timestamp: 2026-02-03T06:35:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue refining the Tejo Beauty home page by further simplifying the Featured Categories section to match the clean, minimal aesthetic.

## Scope

- Update `apps/web/src/components/home/FeaturedCategories.tsx` to remove visual clutter and reduce UI elements.
- Remove icon overlays, gradient color overlays, product count labels, hover arrows, and the quick-link tag pill row.
- Keep the heading/subheading text and category name translations intact.
- Ensure responsive layout remains clean across breakpoints.

## Constraints

- Use existing Tailwind utility classes; do not add new dependencies.
- Maintain current translation keys for headings and category names.
- Keep cards image-driven with subtle overlays for readability.

## Acceptance Criteria

- Featured Categories renders as a clean image grid with only category names.
- No tag pills, icons, gradients, or hover arrows remain in this section.
- TypeScript and lint checks pass for `@tejo/web`.
