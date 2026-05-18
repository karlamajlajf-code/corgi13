# Corgi13 - Tejo Beauty Enterprise Platform

A comprehensive e-commerce platform for Tejo Beauty, built with modern web technologies including React, Next.js, Node.js, and PostgreSQL.

## Project Overview

Corgi13 is a monorepo containing:
- **Frontend**: React/Next.js web application (`tejospec/apps/web`)
- **Backend**: Node.js API with Express/Fastify (`tejospec/apps/api`)
- **Shared**: Utilities, types, and components (`tejospec/packages/shared`)
- **Database**: PostgreSQL with Prisma ORM (`tejospec/database`)
- **Testing**: Playwright E2E tests, Jest unit tests
- **Search**: Elasticsearch integration

## Prerequisites

- **Node.js**: >=20.0.0
- **pnpm**: >=9.0.0 (package manager)
- **PostgreSQL**: 14+ (for database)
- **Elasticsearch**: 8.0+ (for search features)
- **Docker & Docker Compose**: For local development stack

## Quick Start

### 1. Clone with Submodules
```bash
git clone https://github.com/karlamajlajf-code/corgi13.git
cd corgi13
git submodule update --init --recursive
```

### 2. Install Dependencies
```bash
cd tejospec
pnpm install
```

### 3. Environment Setup
```bash
# Copy env template
cp .env.example .env.local

# Bootstrap environment
pnpm run env:bootstrap
```

### 4. Start Development Stack
```bash
# Start Docker services (PostgreSQL, Elasticsearch, Redis)
docker-compose up -d

# Run database migrations
pnpm run db:migrate

# Start development servers (both web and API)
pnpm run dev

# Or start individually:
pnpm run dev:web   # Frontend on http://localhost:3003
pnpm run dev:api   # API on http://localhost:3000
```

### 5. Run Tests
```bash
# Unit tests
pnpm test

# E2E tests
pnpm run test:e2e

# Lint & format
pnpm run lint
pnpm run format
```

## Project Structure

```
corgi13/
├── tejospec/                  # Main monorepo root
│   ├── apps/
│   │   ├── api/              # Node.js backend API
│   │   ├── web/              # React/Next.js frontend
│   ├── packages/
│   │   ├── shared/           # Shared utilities & types
│   ├── database/
│   │   ├── prisma/           # Database schema & migrations
│   │   ├── elasticsearch/    # Search configuration
│   ├── scripts/              # Development scripts
│   ├── tests/                # E2E test suites
│   ├── playwright.config.ts  # Playwright configuration
│   ├── package.json          # Root package.json
│   ├── pnpm-workspace.yaml   # pnpm workspaces config
│
├── docs/                     # Project documentation
├── infrastructure/           # Deployment configs
├── .github/workflows/        # GitHub Actions CI/CD
```

## Available Scripts

From the `tejospec` directory:

```bash
# Development
pnpm run dev              # Start all services
pnpm run dev:web          # Frontend only
pnpm run dev:api          # API only
pnpm run live             # Live development with hot reload
pnpm run smoke:live       # Smoke tests in dev mode
pnpm run smoke:wholesale  # B2B wholesale smoke tests

# Testing
pnpm test                 # Jest unit tests
pnpm run test:e2e         # Playwright E2E tests
pnpm run lint             # ESLint
pnpm run format           # Prettier formatting
pnpm run type:check       # TypeScript type checking

# Database
pnpm run db:migrate       # Run migrations
pnpm run db:seed          # Seed database
pnpm run db:reset         # Reset & reseed

# Building
pnpm run build            # Build all packages
pnpm run build:web        # Build frontend
pnpm run build:api        # Build API
```

## Environment Variables

Key environment variables (see `.env.example`):

```
# Database
DATABASE_URL=postgresql://user:password@localhost:5432/tejo

# API
API_PORT=3000
API_HOST=localhost

# Frontend
NEXT_PUBLIC_API_URL=http://localhost:3000
NEXT_PUBLIC_APP_URL=http://localhost:3003

# Elasticsearch
ELASTICSEARCH_URL=http://localhost:9200

# Search & Recommendations
SEARCH_INDEX_NAME=products
ML_RECOMMENDATIONS_ENABLED=true
```

## API Documentation

API endpoints are documented via OpenAPI specs in `tejospec/openapi/`. Access Swagger UI at:
```
http://localhost:3000/api/docs
```

## Database Migrations

Migrations are managed with Prisma:

```bash
# Create a new migration
pnpm run db:create-migration <name>

# Run pending migrations
pnpm run db:migrate

# Reset database (warning: destructive)
pnpm run db:reset
```

## Testing Strategy

- **Unit Tests**: Jest with `@testing-library`
- **E2E Tests**: Playwright with multi-browser support (Chromium, Firefox, WebKit)
- **Integration Tests**: API tests with real database
- **Smoke Tests**: Quick health checks for deployments

Run all tests:
```bash
pnpm run verify  # Unit + E2E + Lint
```

## Git Workflow

1. Create a feature branch: `git checkout -b feature/feature-name`
2. Make commits with conventional format: `feat: description`, `fix: description`
3. Push and create a pull request
4. CI/CD runs automatically
5. Merge after review

## Troubleshooting

### Port Already in Use
```bash
# Find process on port
lsof -i :3000  # or :3003, :5432, etc.
# Kill process
kill -9 <PID>
```

### Database Connection Issues
```bash
# Check PostgreSQL is running
docker ps | grep postgres

# View logs
docker logs <container_id>

# Reset database
docker-compose down -v
docker-compose up -d
pnpm run db:migrate
```

### Dependency Issues
```bash
# Clear pnpm cache
pnpm store prune

# Reinstall dependencies
rm -rf node_modules pnpm-lock.yaml
pnpm install
```

## CI/CD Pipeline

GitHub Actions workflows run on:
- **Push to main**: Run tests, lint, type-check, build
- **Pull requests**: Same checks + coverage reports
- **Scheduled**: Nightly E2E tests, security scans

See `.github/workflows/` for configurations.

## Documentation

- `docs/` - Architecture, guides, and project documentation
- `tejospec/README.md` - Detailed monorepo documentation
- API docs - OpenAPI specs in `tejospec/openapi/`

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) for guidelines on:
- Code style and conventions
- Commit message format
- Pull request process
- Testing requirements

## License

Proprietary - All rights reserved

## Support

For issues and questions:
- GitHub Issues: https://github.com/karlamajlajf-code/corgi13/issues
- Discussions: https://github.com/karlamajlajf-code/corgi13/discussions

## Team

Built by the Tejo Beauty Development Team

---

**Last Updated**: May 18, 2026
