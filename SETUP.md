# Development Setup Guide

This guide walks you through setting up the Tejo Beauty Enterprise Platform for local development.

## Prerequisites

Before you start, ensure you have:

- **Node.js 20+**: [https://nodejs.org/](https://nodejs.org/)
- **pnpm 9.0+**: `npm install -g pnpm@latest`
- **Git**: [https://git-scm.com/](https://git-scm.com/)
- **Docker & Docker Compose**: [https://www.docker.com/products/docker-desktop](https://www.docker.com/products/docker-desktop)

Verify your setup:

```bash
node --version  # Should be v20.x.x or higher
pnpm --version  # Should be 9.x.x or higher
docker --version
docker compose --version
```

## Step 1: Clone the Repository

```bash
git clone https://github.com/karlamajlajf-code/corgi13.git
cd corgi13
git submodule update --init --recursive
```

## Step 2: Install Dependencies

```bash
cd tejospec
pnpm install
```

This installs all dependencies for the monorepo including frontend, backend, and shared packages.

## Step 3: Environment Setup

The project needs environment variables. Bootstrap them automatically:

```bash
pnpm run env:bootstrap
```

This creates necessary `.env` files with sensible defaults. Review and customize as needed:

- `apps/api/.env` - API configuration
- `apps/web/.env.local` - Frontend configuration
- `database/.env` - Database credentials

## Step 4: Database Setup (Optional)

To run the full stack with PostgreSQL and Elasticsearch:

```bash
# Start database containers
pnpm run db:up

# Run migrations
pnpm run api:migrate

# Seed with sample data
pnpm run api:seed
```

Or do all at once:

```bash
pnpm run db:bootstrap
```

## Step 5: Verify Setup

Run the health check:

```bash
pnpm run tejo:doctor
```

Expected output should show "OK" for most items (Docker warnings are fine if not installed).

## Step 6: Start Development

### Option A: Start Everything

```bash
pnpm run live
```

This starts:

- Frontend: http://localhost:3003
- API: http://localhost:3000
- Watches for changes

### Option B: Start Individually

In separate terminals:

```bash
# Terminal 1: Frontend
pnpm run dev:web

# Terminal 2: API
pnpm run dev:api

# Terminal 3: Database (if using)
pnpm run db:up
```

### Option C: Minimal Setup (No Database)

If you don't need the database:

```bash
pnpm run dev
```

## Common Tasks

### Run Tests

```bash
# Unit tests
pnpm test

# E2E tests (requires running servers)
pnpm run test:e2e

# All checks (lint + type check + test)
pnpm run verify
```

### Code Quality

```bash
# Run ESLint
pnpm run lint

# Format code with Prettier
pnpm run format

# Type checking
npx tsc --noEmit
```

### Database Migrations

```bash
# Create new migration
pnpm run api:create-migration AddFeatureX

# Run migrations
pnpm run api:migrate

# Reset database (destructive!)
pnpm run db:reset
```

### Smoke Tests

Quick verification that everything works:

```bash
# Basic smoke test
pnpm run smoke:live

# With Stripe integration
pnpm run smoke:stripe:proxy:strict

# Wholesale B2B flow
pnpm run smoke:wholesale:ci
```

## Troubleshooting

### Port Already in Use

```bash
# Find and kill process on port
# macOS/Linux:
lsof -i :3000 | grep -v PID | awk '{print $2}' | xargs kill -9

# Windows PowerShell:
netstat -ano | findstr :3000
taskkill /PID <PID> /F
```

### Database Connection Issues

```bash
# Check containers are running
docker ps

# View logs
docker logs <container_id>

# Restart database
pnpm run db:reset
```

### Dependency Conflicts

```bash
# Clear pnpm store
pnpm store prune

# Reinstall
rm -rf node_modules pnpm-lock.yaml
pnpm install
```

### TypeScript Errors

```bash
# Clear TypeScript cache
rm -rf .nx/cache

# Reinstall TypeScript
pnpm add -D typescript@latest
```

## Development Workflow

1. **Create a branch**: `git checkout -b feature/my-feature`
2. **Make changes** and test locally
3. **Run quality checks**: `pnpm run verify`
4. **Commit with conventional message**: `git commit -m "feat: description"`
5. **Push and create PR**: `git push origin feature/my-feature`

## Documentation

- [README.md](./README.md) - Project overview
- [CONTRIBUTING.md](./CONTRIBUTING.md) - Contribution guidelines
- [tejospec/README.md](./tejospec/README.md) - Monorepo documentation
- API Docs: Run the API and go to http://localhost:3000/api/docs

## Getting Help

- Check [GitHub Issues](https://github.com/karlamajlajf-code/corgi13/issues)
- Review [docs/](./docs/) directory for architecture guides
- Open a [GitHub Discussion](https://github.com/karlamajlajf-code/corgi13/discussions)

## Next Steps

1. ✅ Environment set up
2. ✅ Dependencies installed
3. 📖 Review [CONTRIBUTING.md](./CONTRIBUTING.md) for code standards
4. 🚀 Start building!

---

For questions or issues, open a GitHub issue or discussion. Happy coding! 🎉
