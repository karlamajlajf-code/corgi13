# Task Completion Checklist

When completing a development task in Tejo Beauty, follow these steps to ensure quality and consistency:

## 1. Code Quality

### Type Safety
- [ ] Remove all `any` types, use proper type annotations
- [ ] Add explicit return types to exported functions
- [ ] Validate all input data with Zod or class-validator
- [ ] Use Prisma types for database entities

### Linting & Formatting
```bash
# Run ESLint (fix auto-fixable issues)
pnpm lint

# Format with Prettier
pnpm format

# Type check
pnpm typecheck
```

### Code Review Self-Check
- [ ] No console.log statements (use proper logging)
- [ ] No commented-out code (remove or uncomment)
- [ ] Descriptive variable/function names
- [ ] Complex logic has explanatory comments
- [ ] No magic numbers (use named constants)

## 2. Testing

### Unit Tests
```bash
# Run unit tests for the changed module
pnpm test

# Watch mode during development
pnpm test:watch

# Check coverage
pnpm test:coverage
```

- [ ] Write tests for new functions/components
- [ ] Update existing tests if behavior changed
- [ ] Aim for >80% code coverage
- [ ] Test edge cases and error scenarios

### E2E Tests (if applicable)
```bash
# Run Playwright tests
pnpm test:e2e

# Run in UI mode for debugging
pnpm test:e2e:ui
```

- [ ] Add E2E test for new user flows
- [ ] Verify critical paths still work

## 3. Database Changes

If you modified `schema.prisma`:
```bash
# Apply migration
pnpm api:migrate

# Regenerate Prisma client
pnpm api:generate

# Update seed data if needed
pnpm api:seed

# Verify migration worked
pnpm db:up
# Check database in Prisma Studio
```

- [ ] Migration applied successfully
- [ ] Prisma client regenerated
- [ ] Seed data updated (if applicable)
- [ ] No data loss in migration

## 4. API Changes

If you added/modified API endpoints:
- [ ] Update OpenAPI/Swagger documentation
- [ ] Add request/response DTOs with validation
- [ ] Implement proper error handling
- [ ] Add rate limiting if needed
- [ ] Test endpoint manually (Postman/Thunder Client)
- [ ] Update API client types (frontend)

```bash
# Verify Swagger docs
# Open http://localhost:8003/api/docs
pnpm dev:api
```

## 5. Frontend Changes

If you modified UI components:
- [ ] Test responsive design (mobile, tablet, desktop)
- [ ] Verify accessibility (keyboard navigation, ARIA)
- [ ] Check color contrast (WCAG AA compliance)
- [ ] Test with React DevTools
- [ ] Verify no unnecessary re-renders

Performance checks:
- [ ] Images optimized (use next/image)
- [ ] Components lazy-loaded where appropriate
- [ ] No layout shift (CLS) issues
- [ ] Fast interaction (FID < 100ms)

## 6. Documentation

- [ ] Update relevant README.md files
- [ ] Add JSDoc comments to exported functions
- [ ] Update API documentation (if backend changes)
- [ ] Add inline comments for complex logic
- [ ] Update CHANGELOG.md (if applicable)

## 7. Pre-Commit Checks

Run the comprehensive pre-flight check:
```bash
# Standard pre-flight (recommended)
pnpm tejo:preflight

# Strict mode (CI-level checks)
pnpm tejo:preflight:strict

# Health check
pnpm tejo:doctor
```

This will verify:
- ✅ No uncommitted changes conflict with generated files
- ✅ TypeScript compiles without errors
- ✅ ESLint passes
- ✅ Tests pass
- ✅ Database connectivity (if using --skip-db flag omitted)

## 8. Git Workflow

```bash
# Stage changes
git add .

# Commit (Husky will auto-run lint-staged)
git commit -m "feat(scope): descriptive message"

# Push to branch
git push origin <branch-name>
```

- [ ] Commit message follows convention (feat/fix/refactor/etc.)
- [ ] Branch name is descriptive (feature/fix/refactor-xyz)
- [ ] No sensitive data in commits (API keys, passwords)
- [ ] Pre-commit hooks passed

## 9. Verification

### Local Testing
```bash
# Start full stack
pnpm dev

# Test in browser
# Frontend: http://localhost:3003
# API: http://localhost:8003/api/docs
```

- [ ] Feature works in development environment
- [ ] No console errors or warnings
- [ ] API endpoints respond correctly
- [ ] Database queries execute successfully

### Smoke Tests
```bash
# Run automated smoke tests
pnpm smoke:wholesale

# Verify critical flows
pnpm verify:wholesale
```

## 10. Pull Request Checklist

Before creating PR:
- [ ] All above checks passed
- [ ] Branch is up-to-date with main/master
- [ ] No merge conflicts
- [ ] PR description explains changes
- [ ] Screenshots added (if UI changes)
- [ ] Breaking changes documented (if any)

---

## Quick Checklist (TL;DR)

```bash
# 1. Code quality
pnpm lint && pnpm format && pnpm typecheck

# 2. Tests
pnpm test

# 3. Database (if changed)
pnpm api:migrate && pnpm api:generate

# 4. Pre-flight
pnpm tejo:preflight

# 5. Commit
git add . && git commit -m "type(scope): message"
```

**Remember:** The goal is high-quality, maintainable code that won't break in production. Take the time to verify everything works correctly!
