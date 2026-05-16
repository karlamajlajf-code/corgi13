---
id: std-20251228-173500-tejo-wholesale-smoke-test
timestamp: 2025-12-28T17:35:00Z
executed: false
originalPrompt: "go"
---

# Improved Prompt

## Objective

Validate (and document) the end-to-end Tejo wholesale catalog request funnel so it is production-ready for outreach traffic.

## Scope

- Confirm the web funnel pages render and submit correctly:
  - `/wholesale`
  - `/wholesale/catalog-request`
- Confirm the API endpoint works with the global prefix:
  - `POST /api/v1/wholesale/catalog-requests`
- Confirm persistence is working with Prisma:
  - `WholesaleCatalogRequest` records are created
- Ensure the web API client correctly targets `/api/v1` across local + docker environments.

## Constraints

- Avoid destructive DB operations unless explicitly requested.
- Prefer Nx tasks for checks (`nx run ...`) over running underlying tools directly.

## Acceptance Criteria

- Submitting the catalog request form returns `{ success: true, id }`.
- A corresponding `WholesaleCatalogRequest` row exists in Postgres.
- No TypeScript errors in `@tejo/web` and `@tejo/api` (`nx run ...:typecheck` passes).

## Implementation Notes

- Web API client base URL must normalize to `/api/v1` even if `NEXT_PUBLIC_API_URL` is set to host-only (e.g. `http://backend:8003`).
- API must remain prefixed by `api/v1` (as set in Nest bootstrap).
