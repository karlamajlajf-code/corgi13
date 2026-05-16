---
id: std-20251228-153000-tejo-wholesale-catalog-funnel
timestamp: 2025-12-28T15:30:00Z
executed: true
originalPrompt: "go (implement the wholesale catalog request funnel)"
---

# Improved Prompt

## Objective

Implement a public wholesale (B2B) lead capture funnel that turns outbound outreach into measurable inbound catalog requests.

## Scope

### Web (Next.js Pages Router)

- Add a public wholesale landing page at `/wholesale`.
- Add a public catalog request page at `/wholesale/catalog-request`:
  - Form fields: business name, country, city, business type, contact name, email, optional phone/WhatsApp, optional VAT ID, optional monthly purchase range, optional restock interests.
  - Consent checkbox required.
  - Client-side validation and friendly error messages.
  - On submit, POST to the API and show a success state with next steps.
- Add navigation + CTAs:
  - Add “Wholesale (B2B)” entry in the header quick links.
  - Update the wholesale tiers page to route users into the catalog request funnel.

### API (NestJS + Prisma)

- Add a new Prisma model to persist inbound catalog requests.
- Add a public endpoint:
  - `POST /api/v1/wholesale/catalog-requests`
  - Validates request body.
  - Persists the request in Postgres via Prisma.
  - Stores lightweight metadata (ip/user-agent/referer) in a JSON column.
  - Returns `{ success: true, id }`.
- Ensure the module is imported in `AppModule`.

## Constraints

- Keep changes in canonical apps only: `tejospec/apps/web` and `tejospec/apps/api`.
- Follow existing styling patterns (Tailwind + lucide-react), and existing API base URL conventions.
- Keep the endpoint public (no auth required).

## Acceptance Criteria

- Visiting `/wholesale` and `/wholesale/catalog-request` renders correctly.
- Form validates required fields and submits successfully.
- API endpoint exists at `/api/v1/wholesale/catalog-requests` and saves to DB.
- Wholesale tiers CTAs and header quick link drive traffic to catalog request.
- Typechecks pass for `@tejo/web` and `@tejo/api`, and Prisma client is regenerated.
