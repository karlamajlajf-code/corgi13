---
id: std-20251226-000500-ml-recommendations-backend
timestamp: 2025-12-26T00:05:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt - ML Recommendations Backend (tejospec)

## Objective

Implement a recommendations backend in the tejospec NestJS API to serve personalized, similar, and trending product suggestions using existing Prisma data (orders, wishlist, product metrics).

## Scope

- Add new Nest module `RecommendationsModule` under apps/api/src/modules/recommendations with controller + service.
- Endpoints:
  - GET /recommendations/personal (auth required via JwtAuthGuard), optional limit (default 12, max 50).
  - GET /recommendations/similar/:productId (public), optional limit (default 8, max 30).
  - GET /recommendations/trending (public), optional limit (default 12, max 50).
- Data sources and heuristics:
  - Personal: co-purchase/co-occurrence from OrderItems of other users who bought items the current user bought; include wishlist products as seed; exclude already owned/seed products; fallback to trending when sparse.
  - Similar: same category + optional same brand + overlapping tags; exclude the anchor product.
  - Trending: order by salesCount, viewCount, rating, recency; only ACTIVE products.
- Response shape: { data: Product[], meta: { count, fallback?: 'trending' | 'empty' | 'collaborative' } } with include { category, brand, variants, media(order asc) }.
- Register module in AppModule; keep Swagger tags/descriptions consistent with existing controllers.

## Constraints

- Use existing PrismaService; no new deps.
- Use JwtAuthGuard for personal route; derive userId from request (payload userId/sub).
- Enforce limit bounds; ignore/404 when product not found; throw NotFoundException in service.
- Exclude non-ACTIVE products; ignore drafts/archived/out_of_stock.
- Keep code typed (no any); stay ASCII; keep comments minimal.
- Follow Nx/Nest conventions used in products/blog modules.

## Acceptance Criteria

- All three endpoints return 200 with described shape; personal returns 401 without auth; similar returns 404 for missing product.
- Personal route falls back to trending when no signals; meta.fallback reflects fallback cause.
- Sorting reflects heuristic: collaborative counts primary; rating/sales/view secondary; trending sorted by sales/view/rating/createdAt.
- AppModule imports RecommendationsModule; build via `pnpm nx run @tejo/api:build` passes.
- No lint errors about unused vars or img tags; only ACTIVE products returned.
