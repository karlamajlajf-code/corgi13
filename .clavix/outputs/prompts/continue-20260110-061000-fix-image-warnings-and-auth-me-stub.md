---
id: continue-20260110-061000-fix-image-warnings-and-auth-me-stub
timestamp: 2026-01-10T06:10:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Keep the default Playwright E2E run green while reducing recurring console noise by:

- Eliminating remaining Next.js `next/image` warnings for `<Image fill />` missing the `sizes` prop.
- Making auth flows deterministic in `user-authentication.spec.ts` by stubbing `GET/PATCH **/api/auth/me` to return stable 200 responses (instead of producing 401 noise).

## Scope

- Add `sizes` props to remaining `<Image ... fill ...>` usages missing `sizes`, specifically:

  - `apps/web/src/components/home/Testimonials.tsx`
  - `apps/web/src/pages/blog/[slug].tsx`
  - `apps/web/src/pages/admin/blog/new.tsx`
  - `apps/web/src/pages/admin/blog/edit/[id].tsx`

- Update `tests/e2e/specs/user-authentication.spec.ts`:
  - Replace the current `/api/auth/me` route handler that returns 401 with the same stable stubbing pattern used in `account-pages.spec.ts` (200 user profile on GET; 200 ok on PATCH).

## Constraints

- Prefer fixing warnings at the source (adding `sizes`) rather than suppressing console output.
- Do not introduce locale-dependent E2E selectors.
- Keep changes minimal and low-risk.

## Acceptance Criteria

- Running the key affected E2E tests (`product-browsing` and `should allow user to log in`) does not emit:

  - `Image ... has "fill" but is missing "sizes" prop`
  - `Failed to load resource: the server responded with a status of 401 (Unauthorized)`

- Tests still pass via Nx:
  - `pnpm nx run @tejo/root:e2e -- --project=chromium --grep="product browsing"`
  - `pnpm nx run @tejo/root:e2e -- --project=chromium --grep="should allow user to log in"`
