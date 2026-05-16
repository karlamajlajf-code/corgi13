# Tejo Beauty - Enterprise E-Commerce Platform

## Project Purpose
Tejo Beauty is a luxury beauty e-commerce platform featuring an advanced microservices architecture. The platform provides:
- Premium beauty product catalog and shopping experience
- Wholesale/B2B functionality for enterprise customers
- Advanced features: AR try-on, personalization, voice commerce
- Real-time analytics and ML-powered recommendations

## Tech Stack

### Frontend
- **Framework:** Next.js 14.2.35 (App Router)
- **UI Library:** React 18.3.1 with Framer Motion, Lucide React icons
- **State Management:** Zustand 5.0.4 + TanStack Query 5.64.2
- **Forms:** React Hook Form 7.54.2 + Zod 3.24.4 validation
- **Styling:** Tailwind CSS 3.4.17
- **i18n:** next-i18next 15.4.2 + i18next 24.2.3
- **Port:** 3003 (dev)

### Backend
- **Framework:** NestJS 10.4.0 with Express
- **Database:** PostgreSQL (via Docker) + Prisma ORM 5.18.0
- **Search:** Elasticsearch 9.2.0
- **Authentication:** Passport JWT + Local strategies
- **Security:** Helmet, Throttler (rate limiting), bcryptjs
- **API Docs:** Swagger/OpenAPI
- **Port:** 8003 (dev)

### Shared
- **Package:** @tejo/shared (workspace package for shared types/utils)

### Infrastructure
- **Monorepo:** Nx 22.3.3 + pnpm 9.14.2 workspace
- **Node Version:** 20.0.0+ (LTS)
- **Container:** Docker + Docker Compose
- **CI/CD:** GitHub Actions
- **Testing:** Jest (unit) + Playwright (e2e)
- **Monitoring:** Prometheus, Grafana, Jaeger

## Architecture
- **Monorepo structure** with Nx workspace management
- **Canonical apps:** `apps/web` (Next.js) + `apps/api` (NestJS)
- **Legacy apps:** `frontend/`, `backend/` directories (being phased out)
- **Microservices:** ML, Voice, AR, Blockchain services
- **Message Queue:** RabbitMQ
- **Object Storage:** MinIO (S3-compatible)
- **Vector DB:** Qdrant for semantic search

## Key Directories
```
tejospec/
├── apps/
│   ├── web/          # Canonical Next.js frontend (Port 3003)
│   └── api/          # Canonical NestJS API (Port 8003)
├── packages/         # Shared packages
├── backend/          # Legacy backend (being migrated)
├── frontend/         # Legacy frontend (being migrated)
├── database/         # DB schemas, migrations, scripts
├── microservices/    # ML, Voice, AR, Blockchain services
├── infrastructure/   # Docker, K8s configs
├── docs/             # Project documentation
└── tests/            # E2E tests
```

## Design Philosophy
- **Luxury aesthetic:** Gold (#D4AF37) + Black/White palette
- **Typography:** Playfair Display (headings) + Inter (body)
- **Performance-first:** Target Lighthouse >90, LCP <2.5s
- **Type-safe:** Strict TypeScript, Zod validation, Prisma types
- **Secure:** Rate limiting, input sanitization, helmet security headers
