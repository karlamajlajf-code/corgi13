# Phase 1 Implementation Prompt: Tejo Beauty Branding + i18n + Admin Verify Enablement

**Date**: 2026-01-04

## Objective

Ship the Phase 1 foundation work for Tejo Beauty in the canonical Nx workspace under `tejospec/`, ensuring:

- Branding is centralized and used in UI ("Tejo Beauty", `tejobeauty.com` emails/domains).
- i18n defaults to Croatian (`hr`) consistently (config + SSR fallbacks), with language switching that persists.
- B2B/wholesale schema is confirmed present in the canonical Prisma schema and seeded appropriately.
- Wholesale verification can run optional admin-inbox checks when credentials are provided.

## Non-Negotiable Constraints

- Canonical source of truth is `tejospec/apps/api` (NestJS) and `tejospec/apps/web` (Next.js Pages Router).
- Do not implement new functionality under legacy roots (`tejospec/backend/**`, `tejospec/frontend/**`, `tejospec/archive/**`).
- Prefer Nx targets (`pnpm exec nx run ...`) for typecheck/test.

## Scope

### 1) Branding

- Ensure the shared branding constants exist in `tejospec/packages/shared` and are exported from `@tejo/shared`.
- Update `@tejo/web` components to consume branding constants where appropriate (Header/Footer, etc.).
- Confirm no lingering `tejonails.com`/"Tejo Nails" literals remain in canonical app code.

### 2) i18n foundation

- Ensure `next-i18next` config default locale is `hr` and SSR translation calls fall back to `hr` (not `de`).
- Ensure the language switcher persists selection (cookie + localStorage) and respects prior selection on load.
- Keep namespace usage backwards-compatible (current code uses `common`).

### 3) B2B schema (confirm + seed)

- Confirm canonical Prisma schema includes wholesale tiering/pricing/partner profiles.
- Ensure seed data (if present) includes baseline wholesale tier pricing/discount rows.

### 4) Admin inbox verification enablement

- Confirm `pnpm verify:wholesale:strict` can run optional admin inbox checks when admin token/creds are provided.
- Document required env vars and behavior when not provided.

## Acceptance Criteria

- Branding constants are importable from `@tejo/shared` and used in `@tejo/web` logo/metadata where reasonable.
- All `serverSideTranslations(locale ?? 'de', ...)` fallbacks in `@tejo/web` are aligned to `hr`.
- Language selection persists across navigation/reloads.
- Wholesale schema is present in `tejospec/apps/api/prisma/schema.prisma`; seeds exist or are documented.
- Admin inbox checks remain optional by default but run when env is supplied.
- `pnpm exec nx run @tejo/web:typecheck` and `pnpm exec nx run @tejo/api:typecheck` succeed.

## Verification Commands

Run from `tejospec/`:

- `pnpm check:working-tree:strict`
- `pnpm verify:wholesale:strict`
- `pnpm exec nx run @tejo/api:typecheck`
- `pnpm exec nx run @tejo/web:typecheck`

## Notes

If admin-inbox checks are desired, set `SMOKE_ADMIN_TOKEN` (or the documented credential env vars) before running `pnpm verify:wholesale:strict`.
