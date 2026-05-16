---
id: std-20260203-070500-tejo-clean-header-footer
timestamp: 2026-02-03T07:05:00Z
executed: false
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Continue refining the Tejo Beauty experience by simplifying the global Header and Footer to match the clean, minimal aesthetic.

## Scope

- Update `apps/web/src/components/layout/Header.tsx` to remove the top promo bar and quick-link badges, keeping a clean logo, search, and primary category navigation.
- Update `apps/web/src/components/layout/Footer.tsx` to remove the footer newsletter, trust bar, and payment badges while keeping brand description, primary link columns, and legal links.
- Preserve existing translation keys for retained labels.

## Constraints

- Do not remove essential navigation links or break existing routes.
- Use existing Tailwind utility classes only.
- Maintain responsive behavior for mobile and desktop layouts.

## Acceptance Criteria

- Header no longer renders the promotional top bar or quick-link badge row.
- Footer no longer renders the newsletter, trust bar, or payment methods blocks.
- TypeScript and lint checks pass for `@tejo/web`.
