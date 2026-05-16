# Tejo Beauty - Essential Commands Reference

## Development Commands

### Start Services
```bash
# Start canonical web app (Next.js on port 3003)
pnpm dev:web

# Start canonical API (NestJS on port 8003)
pnpm dev:api

# Start both in parallel
pnpm dev

# Start legacy Docker stack (if needed)
pnpm stack:up
pnpm stack:logs
pnpm stack:stop
pnpm stack:down
```

### Database Management
```bash
# Start PostgreSQL + Redis
pnpm db:up

# Stop database services
pnpm db:stop

# Reset database (drop + recreate)
pnpm db:reset

# Full bootstrap (up + migrate + generate + seed)
pnpm db:bootstrap

# Run Prisma migrations
pnpm api:migrate

# Generate Prisma client
pnpm api:generate

# Seed database with sample data
pnpm api:seed

# Open Prisma Studio
cd apps/api && pnpm prisma:studio
```

### Build & Deploy
```bash
# Build all apps
pnpm build

# Build specific app
pnpm build:web
pnpm build:api

# Start production server
pnpm start
```

### Code Quality
```bash
# Lint all projects
pnpm lint

# Format code with Prettier
pnpm format

# Type check all projects
pnpm typecheck

# Run tests
pnpm test

# Run E2E tests (Playwright)
pnpm test:e2e
```

### Verification & Smoke Tests
```bash
# Run Tejo Doctor (health check)
pnpm tejo:doctor
pnpm tejo:doctor:strict

# Pre-flight checks before commit
pnpm tejo:preflight
pnpm tejo:preflight:strict

# Check working tree status
pnpm check:working-tree

# Smoke test live environment
pnpm smoke:live

# Smoke test wholesale flow
pnpm smoke:wholesale

# Verify wholesale functionality
pnpm verify:wholesale
pnpm verify:wholesale:strict
```

### Nx Commands (Preferred)
```bash
# Run task with Nx (cached, optimized)
nx run <project>:<target>

# Run task for multiple projects
nx run-many --target=<target> --all

# Run affected tasks only
nx affected --target=<target>

# View dependency graph
nx graph
```

### System Utilities (Windows)
```bash
# List files/directories
dir
Get-ChildItem  # PowerShell
ls             # Git Bash/WSL

# Find files
where <file>   # Windows
Get-ChildItem -Recurse -Filter <pattern>  # PowerShell

# Search in files (grep equivalent)
findstr /s /i <pattern> *.*  # Windows
Select-String -Pattern <pattern> -Path *  # PowerShell

# Change directory
cd <path>

# Git operations
git status
git add .
git commit -m "message"
git push
```

## Project-Specific Workflows

### After Pulling Changes
```bash
pnpm install          # Install dependencies
pnpm api:generate     # Regenerate Prisma client
pnpm typecheck        # Verify types
```

### Before Committing
```bash
pnpm tejo:preflight   # Run pre-commit checks
# Husky will auto-run lint-staged on commit
```

### When Adding Database Changes
```bash
# Edit schema: apps/api/prisma/schema.prisma
pnpm api:migrate      # Apply schema changes
pnpm api:generate     # Update Prisma client
pnpm api:seed         # Re-seed if needed
```

## Package Manager Notes
- **Primary:** pnpm (required for workspace features)
- **Installation:** npm install -g pnpm
- **Workspace:** Uses pnpm-workspace.yaml for monorepo
- **Scripts:** All pnpm commands support --filter flag for targeting specific apps
