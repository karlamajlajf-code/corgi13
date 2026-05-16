---
id: std-20251228-140000-tejo-wholesale-catalog-request-implement
timestamp: 2025-12-28T14:00:00Z
executed: false
originalPrompt: "go"
---

# Improved Prompt

## Objective

Implement a public **Wholesale Catalog Request** conversion flow in the canonical Tejo stack so outbound outreach can drive measurable inbound B2B leads.

## Scope

### Web (`tejospec/apps/web`)

1. Add a public wholesale entry page:

   - Route: `/wholesale` (create `src/pages/wholesale/index.tsx`)
   - Purpose: explain wholesale value (Croatia-based EU importer, operational trust, reorders) and drive a single CTA to request a catalog.

2. Add a public catalog request landing + form:

   - Route: `/wholesale/catalog-request` (create `src/pages/wholesale/catalog-request.tsx`)
   - Use the fields/copy guidance from `tejospec/docs/growth/CATALOG_REQUEST_LANDING_PAGE.md`.
   - Required fields: businessName, country, city, businessType, contactName, email
   - Optional fields: phoneWhatsapp, vatId, monthlyPurchaseRange, restockInterests
   - Consent microcopy + clear success confirmation state.

3. Add entry points:

   - Update `src/components/layout/Header.tsx` to expose a visible “Wholesale / B2B” link (desktop + mobile).
   - Update `src/pages/wholesale/tiers.tsx` to include a CTA button linking to `/wholesale/catalog-request`.

4. API wiring notes:
   - The web app should submit to a backend endpoint via `fetch('/api/...')` using JSON.
   - Handle network errors and show a human-friendly message.

### API (`tejospec/apps/api`)

1. Add a Prisma model to store catalog requests:

   - Create a new model (e.g. `WholesaleCatalogRequest`) in `prisma/schema.prisma` with:
     - businessName, country, city, businessType, contactName, email
     - optional: phoneWhatsapp, vatId, monthlyPurchaseRange, restockInterests
     - optional: metadata Json (user agent / referrer), status string default `NEW`
     - createdAt/updatedAt + index on `email` and `createdAt`

2. Add a new Nest module using existing patterns (Injectable service + PrismaService):

   - New module folder under `src/modules/` (e.g. `catalog-requests/`).
   - Controller path should live under the wholesale marketing namespace:
     - `POST /wholesale/catalog-requests`
   - Validate inputs using `class-validator` DTO.
   - Persist request via Prisma.
   - Return `{ success: true, id: ... }`.

3. Wire into app:
   - Import the new module in `src/app.module.ts`.

## Constraints

- Keep changes within canonical apps (`tejospec/apps/web`, `tejospec/apps/api`, `tejospec/packages/shared`) and avoid touching legacy `tejospec/frontend`/`tejospec/backend`.
- Keep styling consistent with existing wholesale pages (dark slate + pink accents).
- No deceptive copy or dark patterns.

## Acceptance Criteria

- Visiting `/wholesale` and `/wholesale/catalog-request` works and looks consistent with `wholesale/tiers`.
- Header exposes a discoverable Wholesale entry point.
- Submitting the catalog request form creates a record in the database via the API endpoint.
- API compiles/typechecks after `prisma generate`.
- Web and API pass focused lint/typecheck/tests impacted by the change.
