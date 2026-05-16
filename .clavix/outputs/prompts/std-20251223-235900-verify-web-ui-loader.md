---
id: std-20251223-235900-verify-web-ui-loader
timestamp: 2025-12-23T23:59:00Z
executed: false
originalPrompt: "verify latest @tejo/web UI changes and remove Next.js stylesheet-in-next/head warning"
---

# Improved Prompt

## Objective

Verify the recent Juliana-inspired luxury UI updates in `tejospec/apps/web` and eliminate any warnings/errors introduced by them.

## Scope

- Fix the Next.js warning about adding stylesheets via `next/head` by ensuring Google Fonts are only loaded via `pages/_document.tsx` (or other Next-recommended mechanism), not per-page/head.
- Keep the current global layout wrapper and loader behavior intact.
- Run focused Nx-based verification for the web app.

## Constraints

- Keep changes minimal and localized (don’t refactor unrelated UI).
- Maintain accessibility and responsiveness behavior already implemented.

## Acceptance Criteria

- No more runtime warning: "Do not add stylesheets using next/head".
- `@tejo/web` passes:
  - `nx run @tejo/web:typecheck`
  - `nx run @tejo/web:lint`
  - `nx run @tejo/web:test`
  - `nx run @tejo/web:build`
