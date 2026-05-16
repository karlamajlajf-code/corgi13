---
id: continue-20260104-071000-phase4-mvp-blog-search-stripe
timestamp: 2026-01-04T07:10:00Z
executed: false
originalPrompt: "Continue Phase 4 (all tracks) implementation work from the current audited state."
---

# Improved Prompt

## Objective

Continue Phase 4 “all tracks” work in the canonical Nx workspace (`tejospec/`) by implementing the next highest-impact MVP slices surfaced by the audit:

1. Blog: wire web pages to the existing Blog API routes and harden the API so drafts/unpublished content aren’t publicly accessible.
2. Search: align API search response shape with what the web search UI expects.
3. Subscriptions/Payments: implement the missing Stripe payment-method endpoints the web already calls.

## Scope (Must Do)

### Blog (API)

- Public listing endpoint must not allow fetching DRAFT/ARCHIVED posts via query params.
- Public “get by slug” must not return unpublished posts.
- Add admin-only list endpoint (role-guarded) supporting `status` filters and `sortBy=updatedAt`.
- Add admin-only “get by slug” endpoint (role-guarded) to allow previewing drafts.

### Blog (Web)

- Update all blog list pages (index/category/tag) and admin list page to call the correct API routes:
  - Public: `/api/blog/s` and `/api/blog/s/search`
  - Admin: `/api/blog/admin/list`
- Replace the mock blog detail page with API-backed fetching by slug.
- Render markdown safely (no raw HTML execution).

### Search (API)

- `/search` must return objects compatible with `SearchProduct` used by `useSearch()` in the web app:
  - include `image` (or empty string) and `ratingCount` (alias of `reviewCount`).
  - include `category`, `brand`, `stockQuantity`, etc.

### Stripe (API)

- Add `StripeModule` with endpoints behind `JwtAuthGuard`:
  - `GET /stripe/payment-methods` → returns an array of Stripe payment methods (id/type/card).
  - `DELETE /stripe/payment-methods/:id` → detaches payment method.
- When Stripe is not configured (no `STRIPE_SECRET_KEY`), return an empty list for GET and a safe success response for DELETE (don’t hard-fail the UI).

## Constraints

- Work only in the canonical workspace: `tejospec/`.
- Keep changes small, incremental, and backwards-compatible where practical.
- Do not broaden auth architecture; reuse the existing JWT + RBAC patterns.
- Prefer Nx targets for verification (lint/typecheck/test) for `@tejo/api` and `@tejo/web`.

## Acceptance Criteria

- Blog pages no longer reference `/api/blogs*` routes.
- Public blog API no longer leaks drafts (list-by-status or slug).
- Web blog detail page renders real posts from API.
- Web search results render without runtime errors and show review counts (via `ratingCount`).
- `GET /stripe/payment-methods` and `DELETE /stripe/payment-methods/:id` exist and do not error when Stripe isn’t configured.
- Targeted Nx lint/typecheck/tests pass for the touched apps.
