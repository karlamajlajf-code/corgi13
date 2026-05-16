---
id: phase4-roles-guard-20260104-0001
timestamp: 2026-01-04T00:00:00Z
executed: true
originalPrompt: "Implement shared role-based authorization (guard/decorator) for tejospec API and migrate existing admin-only routes off ad-hoc isAdmin() checks."
---

# Improved Prompt

## Objective

Implement a reusable, consistent role-based authorization mechanism for the canonical TejoSpec NestJS API (`tejospec/apps/api`) and migrate existing admin-only endpoints to use it, replacing scattered `isAdmin()` checks.

## Scope

- Add a `@Roles(...roles)` decorator and `RolesGuard` that:
  - Reads allowed roles via `Reflector` metadata.
  - Uses `req.user.role` (populated by existing JWT strategy) as the source of truth.
  - Compares against Prisma `Role` enum values (uppercase).
  - Throws/returns appropriate NestJS HTTP exceptions (`ForbiddenException`), not generic `Error`.
- Migrate admin-only routes (start with loyalty + blog, then others discovered via search) to:
  - `@UseGuards(JwtAuthGuard, RolesGuard)`
  - `@Roles(Role.ADMIN, Role.SUPER_ADMIN)` (or equivalent policy)
  - Remove local `isAdmin()` helper methods when they become unused.

## Constraints

- Work only in the canonical workspace under `tejospec/`.
- Keep changes minimal, focused, and consistent with existing patterns.
- Preserve passing TypeScript and lint.
- Avoid broad/global auth behavior changes (no global APP_GUARD unless already used).

## Acceptance Criteria

- A shared roles decorator/guard exists and is used by at least the loyalty admin adjust endpoint and blog admin-only endpoints.
- No migrated endpoint uses `throw new Error('Only admins...')`; uses `ForbiddenException` or guard-level denial.
- `pnpm -C tejospec nx lint api` and a focused typecheck/build for API pass (or equivalent existing scripts).

## Suggested Implementation Notes

- Create new files under a clear common/auth location (follow existing repo structure).
- Ensure guard checks both handler and controller metadata.
- Keep explicit return types and avoid `any`.
