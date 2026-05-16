---
id: std-20250126-152504-elasticsearch-advanced-search
timestamp: 2025-01-26T15:25:04Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Implement advanced product search with Elasticsearch integration for tejospec: enable full-text search, faceted filtering, and search-as-you-type functionality with high performance and relevance.

## Scope

### Backend (NestJS API)

- Create ElasticsearchModule with connection configuration using environment variables
- Implement SearchService with methods:
  - indexProduct(product): Index/update single product in Elasticsearch
  - bulkIndexProducts(products): Bulk index multiple products
  - searchProducts(query, filters, pagination): Full-text search with filters
  - getSuggestions(partial): Autocomplete/type-ahead suggestions
  - deleteProduct(id): Remove product from index
- Create SearchController with endpoints:
  - GET /search?q={query}&category={}&brand={}&minPrice={}&maxPrice={}&page={}&limit={}
  - GET /search/suggestions?q={partial}
  - POST /search/reindex (admin only): Trigger full product reindex
- Add Elasticsearch index mapping optimized for:
  - Product name, description, tags (analyzed text)
  - Category, brand, subcategory (keyword for exact match)
  - Price, rating, stock (numeric for range filters)
  - Status (ACTIVE only in search results)
- Integrate with existing ProductsModule to auto-index on product create/update/delete
- Add search analytics tracking (query terms, result counts, click-through)

### Frontend (Next.js)

- Create search results page at `/search` with:
  - Search input with debounced autocomplete dropdown
  - Filter sidebar (category, brand, price range, rating, availability)
  - Results grid with pagination and result count
  - Sort options (relevance, price low-high, price high-low, rating, newest)
  - Loading skeletons and empty states
- Add search bar to Header component with:
  - Autocomplete suggestions as user types
  - Recent searches (localStorage)
  - Quick category shortcuts
  - Keyboard navigation (arrow keys, enter, escape)
- Create useSearch hook for:
  - Search query state management
  - Filter state management
  - Debounced API calls
  - URL query param syncing
- Update product cards to highlight search terms when coming from search

### Infrastructure

- Add Elasticsearch container to docker-compose.canonical-db.yml (port 9200)
- Add Elasticsearch environment variables to apps/api/.env.example
- Create initial index setup script at apps/api/scripts/elasticsearch-setup.ts
- Add reindex command to package.json: `pnpm api:reindex`

## Constraints

- Use @nestjs/elasticsearch package for backend integration
- Keep Elasticsearch optional: if not configured, fallback to basic Prisma LIKE queries
- Maintain existing product filtering API compatibility
- Search must respect product status (ACTIVE only) and user permissions
- Implement proper error handling for Elasticsearch connection failures
- Index only necessary fields to optimize storage and performance
- Use TypeScript strict mode throughout
- Follow existing authentication/authorization patterns for admin endpoints
- Keep frontend responsive and mobile-friendly

## Acceptance Criteria

### Backend

- SearchModule properly configured with Elasticsearch client
- Product index created with optimized mappings for text/keyword/numeric fields
- Search endpoint returns relevant results sorted by score with pagination
- Autocomplete suggestions return within 50ms for typical queries
- Products are automatically indexed/updated/removed when modified via API
- Admin reindex endpoint works and reports progress
- Health check reports Elasticsearch connection status
- API typecheck and build pass without errors

### Frontend

- Search page displays results grid with working filters and pagination
- Autocomplete shows suggestions within 200ms of typing (debounced)
- Filters update URL query params and can be bookmarked/shared
- Sort and filter changes fetch new results without page reload
- Loading states provide feedback during search operations
- Empty search shows helpful message and suggested categories
- Search highlights matched terms in product titles
- Mobile search UI is usable with collapsible filters
- Web typecheck and lint pass without new errors

### Integration

- Elasticsearch container starts with docker-compose and persists data
- Initial product seed creates Elasticsearch indices automatically
- Search performance benchmarks: <100ms for simple queries, <500ms for complex filters
- Fallback to Prisma search works when Elasticsearch is unavailable
- No breaking changes to existing product/category/cart APIs

### Documentation

- README updated with Elasticsearch setup instructions
- API endpoints documented in Swagger with example queries
- Search query syntax examples provided (quotes, wildcards, boolean operators)
