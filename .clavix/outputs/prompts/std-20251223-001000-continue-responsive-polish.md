---
id: std-20251223-001000-continue-responsive-polish
timestamp: 2025-12-23T00:10:00Z
executed: false
originalPrompt: "continue polishing the Juliana-inspired header/hero responsiveness"
---

# Improved Prompt

## Objective

Continue refining the Juliana-inspired luxury UI in `tejospec/apps/web` with a focus on mobile-first responsiveness and accessibility.

## Scope

- Header:
  - Ensure mobile menu and mobile search overlays don’t conflict (opening one closes the other).
  - Add basic focus management for the mobile drawer (focus drawer on open; return focus to trigger on close).
- Hero:
  - Reduce excessive min-height on small screens and use safer viewport sizing so the hero doesn’t overflow on short devices.

## Constraints

- Keep changes minimal and localized to `Header.tsx` and `HeroSection.tsx`.
- Maintain the existing luxury styling and behavior.

## Acceptance Criteria

- Mobile menu/search behave predictably and are keyboard-friendly.
- Hero section looks balanced at ~360px width and short viewport heights.
- `nx run @tejo/web:typecheck` and `nx run @tejo/web:test` pass.
