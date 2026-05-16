# Tejo Beauty - Project Structure & Architecture

## Monorepo Organization

### Workspace Structure (Nx + pnpm)
```
tejospec/
├── apps/                    # Canonical applications (Nx-managed)
│   ├── web/                 # Next.js 14 frontend (Port 3003)
│   └── api/                 # NestJS backend (Port 8003)
│
├── packages/                # Shared workspace packages
│   └── shared/              # @tejo/shared - Common types, utilities, DTOs
│
├── backend/                 # Legacy backend (being migrated to apps/api)
├── frontend/                # Legacy frontend apps (being migrated)
│   ├── web/                 # Legacy Next.js app
│   ├── admin/               # Admin panel (Vite)
│   └── app/                 # Mobile app (React Native)
│
├── microservices/           # Specialized services
│   ├── ml-service/          # ML recommendations (Port 8080)
│   ├── voice-service/       # Voice commerce (Port 8081)
│   ├── ar-service/          # AR try-on (Port 8082)
│   └── blockchain-service/  # Blockchain features (Port 8083)
│
├── database/                # Database related files
│   ├── prisma/              # Prisma schema & migrations
│   ├── elasticsearch/       # ES configs & mappings
│   └── scripts/             # DB utility scripts
│
├── infrastructure/          # DevOps & deployment
│   ├── docker/              # Dockerfiles
│   ├── kubernetes/          # K8s manifests
│   └── terraform/           # Infrastructure as code
│
├── docs/                    # Project documentation
│   ├── MASTER_PLAN.md       # Current canonical architecture
│   ├── api/                 # API documentation
│   └── architecture/        # System design docs
│
├── tests/                   # E2E & integration tests
│   ├── e2e/                 # Playwright tests
│   └── integration/         # API integration tests
│
└── scripts/                 # Build, dev, & utility scripts
    ├── doctor.mjs           # Health check script
    ├── smoke-*.mjs          # Smoke test scripts
    └── verify-*.mjs         # Verification scripts
```

## Key Files

### Root Configuration
- `nx.json` - Nx workspace configuration
- `pnpm-workspace.yaml` - pnpm workspace definition
- `package.json` - Root package with workspace scripts
- `tsconfig.eslint.json` - TypeScript config for ESLint
- `.prettierrc` - Code formatting rules
- `eslint.config.mjs` - ESLint configuration
- `docker-compose.yml` - Local development stack
- `docker-compose.canonical-db.yml` - Canonical DB setup

### Application Configs

**Frontend (apps/web):**
- `next.config.js` - Next.js configuration
- `tailwind.config.js` - Tailwind CSS setup
- `jest.config.ts` - Jest testing config

**Backend (apps/api):**
- `nest-cli.json` - NestJS CLI config
- `prisma/schema.prisma` - Database schema
- `tsconfig.json` - TypeScript compiler options

## Migration Status

### ✅ Canonical (Current)
- `apps/web` - Modern Next.js 14 app on port 3003
- `apps/api` - NestJS backend on port 8003
- pnpm workspace with Nx orchestration

### ⚠️ Legacy (Being Phased Out)
- `backend/` - Old Express backend (migrate to apps/api)
- `frontend/web/` - Old Next.js (migrate to apps/web)
- Multiple SQLite DB paths (consolidating to single source)

### 🎯 Future
- Complete migration to apps/* structure
- Remove legacy directories
- Single source of truth for all configs

## Shared Package (@tejo/shared)

Located in `packages/shared/`, contains:
- **Types:** Common TypeScript interfaces/types
- **DTOs:** Data Transfer Objects for API
- **Utils:** Shared utility functions
- **Constants:** Shared constants/enums

Used by both frontend and backend for type consistency.

## Important Conventions

### Port Assignments
- **3003** - Frontend (apps/web)
- **8003** - API (apps/api)
- **5432** - PostgreSQL
- **6379** - Redis
- **9200** - Elasticsearch
- **5672/15672** - RabbitMQ
- **8080-8083** - Microservices
- **9000** - MinIO (S3)

### Database Locations
- **Dev DB:** `apps/api/prisma/dev.db` (canonical)
- **Legacy:** `backend/prisma/dev.db` (being phased out)

### Environment Variables
- `.env.example` - Template for required env vars
- `.env` - Local environment (gitignored)
- Priority: System env > .env file > defaults

## Dependency Management

### Workspace Dependencies
```json
{
  "@tejo/shared": "workspace:*",  // Always use workspace version
  "@tejo/web": "workspace:*",
  "@tejo/api": "workspace:*"
}
```

### Installation
```bash
pnpm install              # Install all workspace dependencies
pnpm add <pkg>            # Add to root
pnpm --filter @tejo/web add <pkg>  # Add to specific app
```

### Updates
```bash
pnpm update               # Update all packages
pnpm outdated             # Check for outdated packages
```

## Testing Architecture

### Unit Tests (Jest)
- Located alongside source files (`.spec.ts`)
- Run with `pnpm test` or `nx test <project>`

### E2E Tests (Playwright)
- Located in `tests/e2e/`
- Config: `playwright.config.ts`
- Run with `pnpm test:e2e`

### Integration Tests
- API integration tests in `tests/integration/`
- Test full request/response cycles

## Build & Deployment

### Local Development
1. Start DB: `pnpm db:up`
2. Start frontend: `pnpm dev:web`
3. Start backend: `pnpm dev:api`

### Production Build
```bash
pnpm build        # Build all apps
pnpm start        # Start production servers
```

### Docker Deployment
```bash
pnpm stack:up     # Start Docker stack
pnpm stack:logs   # View logs
pnpm stack:down   # Stop stack
```

## Important Notes

⚠️ **Current State:** The project is in active migration from legacy structure to canonical Nx workspace. When working on new features:
- Use `apps/*` directories (canonical)
- Avoid modifying legacy directories unless necessary
- Consult `docs/MASTER_PLAN.md` for architectural guidance
- Run `pnpm tejo:doctor` to verify setup health
