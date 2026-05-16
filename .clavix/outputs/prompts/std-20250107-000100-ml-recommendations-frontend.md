---
id: std-20250107-000100-ml-recommendations-frontend
timestamp: 2025-01-07T00:01:00Z
executed: false
originalPrompt: "Continue by implementing ML recommendations frontend integration"
---

# Improved Prompt

## Objective

Implement the ML recommendations frontend for tejospec: surface personalized/trending/similar product recommendations and replace mock product data with real API responses.

## Scope

- Add frontend API hooks for recommendations (`/recommendations/personal`, `/similar/:id`, `/trending`) and fix the product detail fetch to use the slug endpoint.
- Normalize product payloads (media/images/price) for UI components, providing safe fallback images.
- Update the product detail page to load real product data and render similar recommendations (with trending fallback if empty).
- Update the homepage featured section to render personalized recommendations when authenticated and trending when not.

## Constraints

- Use existing axios client + React Query patterns in `apps/web/src/lib/api/hooks.ts`.
- Keep UI consistent with current design; handle loading/empty/error states gracefully without breaking layout.
- Map Prisma decimal/string numbers to numbers before price formatting; prefer primary media > images array > placeholder.
- Avoid introducing breaking changes to auth or cart flows; keep TypeScript strictness intact by widening types only as needed.

## Acceptance Criteria

- New recommendation hooks return `{ data, meta }` responses and accept optional `limit`; product detail hook targets `/products/slug/:slug`.
- Product detail page shows real product data (name, price, images) and a populated recommendation grid (similar or trending) without runtime errors when API data is missing fields.
- Homepage featured products render API-driven recommendations (personal when logged in, otherwise trending); UI remains styled and responsive.
- Builds/lint compile for updated files; no mock data remains for product detail/recommendation sections.
