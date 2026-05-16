---
id: std-20251226-lint-cleanup
timestamp: 2025-12-26T12:00:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

**Objective:** Clean up the remaining ESLint warnings in @tejo/web by removing unused imports/variables, aligning with Next.js lint rules (replace raw img with next/image), and ensuring focus management cleanup in Header stays lint-compliant. Run lint to confirm a clean report.

**Scope:**

- Update @tejo/web components/pages: home/PromoBanner, home/Testimonials, layout/Header, product/ProductCard.
- Pages: about, account/index, account/orders, account/settings, auth/login, auth/signup-b2b, blog/[slug], blog, careers, cart, checkout/success, checkout, contact, shop/index.
- Apply minimal, non-functional changes: drop unused imports/locals, remove unused state, convert checkout success recommended product images to next/image, capture the menu button ref in Header’s focus effect cleanup.
- Verification: `pnpm nx run @tejo/web:lint`.

**Constraints:**

- No design/behavior changes; only lint-driven cleanups.
- Keep TypeScript types intact unless unused; avoid adding new functionality.
- Preserve existing styling and layout.

**Acceptance Criteria:**

- `pnpm nx run @tejo/web:lint` passes with **0 warnings/errors**.
- Header focus effect cleanup uses a local ref snapshot and keeps behavior unchanged.
- checkout/success uses `next/image` for recommended products; no raw `<img>` remain.
- No new lint warnings introduced.
