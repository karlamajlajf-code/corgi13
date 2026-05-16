---
id: std-20251228-235000-tejo-wholesale-leads-inbox
timestamp: 2025-12-28T23:50:00Z
executed: false
originalPrompt: "CONTINUE BUILDING IT AS I SAID BEFORE!"
---

# Improved Prompt

## Objective

Continue building the Tejo Beauty B2B/wholesale funnel by adding an internal “Leads Inbox” so catalog requests are not only captured, but also reviewed and processed.

## Scope

### Backend (NestJS API)

- Add **admin-only** endpoints for wholesale catalog requests:
  - `GET /api/v1/wholesale/catalog-requests` → list requests (paged), optionally filter by `status` and search by `q` (business/contact/email).
  - `PATCH /api/v1/wholesale/catalog-requests/:id/status` → update `status`.
- Reuse existing auth (`JwtAuthGuard`) and enforce an **admin role check** (accept `ADMIN`, `SUPER_ADMIN`, `MANAGER`).

### Frontend (Next.js web)

- Add an admin page (Pages Router) to view and manage requests:
  - Route: `/admin/wholesale-catalog-requests`
  - Table view with: created date, status, business, contact, email, phone/WhatsApp, country/city, monthly range.
  - Status filter + search box.
  - Inline status update (dropdown) calling the backend `PATCH` endpoint.

### Platform fix (needed for existing admin pages)

- Fix the Next.js rewrite in `apps/web/next.config.js` so frontend fetches to `/api/*` correctly proxy to the backend base URL (no duplicate `/api` segment when `NEXT_PUBLIC_API_URL` already includes `/api/v1`).

## Constraints

- Keep changes minimal and consistent with existing code style.
- Use Nx-preferred commands for verification.
- No new third-party services (email/CRM) in this increment.

## Acceptance Criteria

- Submitting `/wholesale/catalog-request` continues to work.
- Admin can retrieve catalog requests via `GET /api/v1/wholesale/catalog-requests` (requires admin JWT).
- Admin can update a request’s status via `PATCH /api/v1/wholesale/catalog-requests/:id/status`.
- `/admin/wholesale-catalog-requests` renders a list and can update statuses without page reload.
- Fixing the rewrite makes existing `/api/...` fetch patterns stop generating `/api/v1/api/...` URLs.

## Verification

- Run focused Nx lint for affected apps.
- Smoke check:
  - `curl -s http://localhost:8003/api/v1/health | head`
  - (Authenticated) list endpoint returns JSON with `items` and `total`.
