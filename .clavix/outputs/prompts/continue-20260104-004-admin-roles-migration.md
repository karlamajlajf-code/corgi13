---
id: continue-20260104-004-admin-roles-migration
timestamp: 2026-01-04T00:00:00Z
executed: true
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Continue Phase 4 hardening by ensuring all admin-only API endpoints use the shared role-based authorization mechanism (Roles decorator + RolesGuard), and that recent external/formatter edits did not regress access control.

## Scope

- Re-check externally modified files for regressions:
  - `.clavix/outputs/tejo-beauty-enterprise-platform/tasks.md`
  - `tejospec/apps/api/src/modules/loyalty/loyalty.controller.ts`
  - `tejospec/apps/api/src/modules/wholesale-catalog/wholesale-catalog.controller.ts`
- Identify remaining admin-only endpoints (Swagger summaries like “admin only”, routes under `/admin/*`, etc.).
- Migrate remaining controllers (at minimum Users + Search) to:
  - `@UseGuards(JwtAuthGuard, RolesGuard)`
  - `@Roles(Role.MANAGER, Role.ADMIN, Role.SUPER_ADMIN)`
- (Optional but recommended) Make `RolesGuard` use DI for `Reflector` instead of instantiating it directly.
- Update `.clavix` Phase 4 hardening notes to include any newly migrated controllers.

## Constraints

- Work only under canonical `tejospec/`.
- Keep changes minimal and consistent with existing NestJS patterns.
- No broad/global auth changes (avoid global APP_GUARD unless already used).

## Acceptance Criteria

- No controller keeps ad-hoc `isAdmin()`/`assertAdmin()` logic for admin-only endpoints.
- API lint and build pass:
  - `pnpm exec nx run @tejo/api:lint`
  - `pnpm nx build api`
