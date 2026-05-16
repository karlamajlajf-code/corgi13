---
id: continue-20260104-120000-phase4-web-api-base-normalization-search-suggestions
timestamp: 2026-01-04T12:00:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue Phase 4 hardening by fixing two high-impact web integration issues that can cause runtime failures depending on environment configuration:

1. Server-side product page recommendation fetches should always hit the correct API base path (`/api/v1`) even when `NEXT_PUBLIC_API_URL` is set to a bare host.
2. Client-side search suggestions should use the canonical `/api/*` proxy path instead of calling `NEXT_PUBLIC_API_URL` directly (avoids CORS/misconfig issues and keeps behavior consistent with `useSearch()` which already uses the API client).

## Scope (Must Do)

### Web: Product Detail SSR

- In `getServerSideProps` for the product detail page, derive `apiBase` from `API_URL`/`NEXT_PUBLIC_API_URL`/default host and normalize it to include `/api/v1`.
- Keep the existing behavior of fetching:
  - `/products/slug/:slug`
  - `/recommendations/similar/:id?limit=8`
  - `/recommendations/trending?limit=8`

### Web: Search Suggestions

- Replace the direct `fetch(`${process.env.NEXT_PUBLIC_API_URL}/search/suggestions...`)` call with a relative request to `/api/search/suggestions?...`.
- Handle non-OK responses safely (empty suggestions rather than throwing).

## Constraints

- Work only in the canonical workspace: `tejospec/`.
- Keep changes minimal and backwards-compatible.
- Don’t change API contracts; this is web integration hardening.

## Acceptance Criteria

- Search suggestions work even when `NEXT_PUBLIC_API_URL` is unset or points to a host without `/api/v1`.
- Product page SSR recommendation fetches work when `NEXT_PUBLIC_API_URL` is either `http://host` or `http://host/api/v1`.
- `pnpm nx run web:typecheck` passes, and (if available) `pnpm nx run web:lint` passes.
