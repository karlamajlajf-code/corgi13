---
id: std-20251223-023000-smooth-attractive-ui
timestamp: 2025-12-23T02:30:00Z
executed: false
originalPrompt: "make it more smooth and nice, more user attractive"
---

# Improved Prompt

## Objective

Make the `@tejo/web` storefront feel smoother, more premium, and more user-attractive without copying any external brand assets.

## Scope

- Add subtle, consistent micro-interactions:
  - Premium hover/focus states for links/buttons (slight lift, glow, underline).
  - Smoother header feel (subtle translucency + blur when sticky/scrolled).
  - Hero slider: nicer content transitions (fade/slide) while keeping performance.
- Improve loader polish:
  - Entrance loader fade feels smoother and never blocks interaction longer than necessary.
  - Route transition indicator feels more refined.
- Keep accessibility and reduced-motion support.

## Constraints

- Keep changes minimal and localized to existing Tailwind + CSS patterns.
- No heavy new UI frameworks.

## Acceptance Criteria

- Homepage feels noticeably more premium and smooth on mobile + desktop.
- Reduced-motion users don’t get intense animations.
- `nx run @tejo/web:typecheck` and `nx run @tejo/web:test` pass.
