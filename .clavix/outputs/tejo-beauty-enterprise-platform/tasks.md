# Implementation Plan

**Project**: tejo-beauty-enterprise-platform
**Generated**: 2026-01-04

**Spec Source**: `.clavix/outputs/prompts/plan-tejoBeautyEnterprisePlatform.prompt.prompt.md`

## Guardrails

- Canonical source of truth: `tejospec/apps/api` + `tejospec/apps/web`.
- Avoid changes under `tejospec/backend/**`, `tejospec/frontend/**`, `tejospec/archive/**`.
- Prefer Nx targets for validation.

---

## Phase 1: Foundation & Branding

- [x] **Export shared branding config (`@tejo/shared`)**
      Task ID: phase-1-branding-01

  > **Implementation**:
  >
  > - Ensure `tejospec/packages/shared/src/config/branding.ts` is exported via `tejospec/packages/shared/src/index.ts`.
  > - Add a `tejospec/packages/shared/src/config/index.ts` barrel if missing.

- [x] **Use branding constants in key web UI**
      Task ID: phase-1-branding-02

  > **Implementation**:
  >
  > - Update Header/Footer (and other obvious surface areas) to use `BRAND_CONFIG` rather than hardcoded name/domain strings.
  > - Avoid large refactors; keep changes minimal and safe.

- [x] **Align i18n SSR fallbacks to Croatian (`hr`)**
      Task ID: phase-1-i18n-01

  > **Implementation**:
  >
  > - Replace `serverSideTranslations(locale ?? 'de', ...)` with `serverSideTranslations(locale ?? 'hr', ...)` across `tejospec/apps/web/src/pages/**`.

- [x] **Persist language selection (cookie + localStorage)**
      Task ID: phase-1-i18n-02

  > **Implementation**:
  >
  > - Update `tejospec/apps/web/src/components/LanguageSwitcher.tsx` to persist the selected locale.
  > - Ensure initial load respects a previously saved locale when reasonable.

- [x] **Confirm B2B schema + seed coverage**
      Task ID: phase-1-b2b-schema-01

  > **Implementation**:
  >
  > - Confirm wholesale models exist in `tejospec/apps/api/prisma/schema.prisma`.
  > - Locate seed scripts and ensure baseline wholesale tier pricing/discount rows exist (or document how to seed them).

- [x] **Enable admin-inbox verification path (documented env vars)**
      Task ID: phase-1-verify-admin-01

  > **Implementation**:
  >
  > - Confirm verify scripts already support admin mode; if not, add support for `SMOKE_ADMIN_TOKEN`.
  > - Document how to run with admin checks enabled.

---

## Verification (after Phase 1)

- [x] Run `pnpm check:working-tree:strict` (must PASS)
- [x] Run `pnpm verify:wholesale:strict` (must PASS; admin inbox checks may remain optional without creds)
- [x] Run `pnpm exec nx run @tejo/api:typecheck` (must PASS)
- [x] Run `pnpm exec nx run @tejo/web:typecheck` (must PASS)

---

## Phase 2: Premium UX & Animations

- [x] **Create premium UI components (AnimatedBackground / GlowCard / SpotlightCard)**
      Task ID: phase-2-ux-01

  > **Implementation**:
  >
  > - `tejospec/apps/web/src/components/ui/AnimatedBackground.tsx`
  > - `tejospec/apps/web/src/components/ui/GlowCard.tsx`
  > - `tejospec/apps/web/src/components/ui/SpotlightCard.tsx`

- [x] **Add route transitions via Layout wrapper**
      Task ID: phase-2-ux-02

  > **Implementation**:
  >
  > - `tejospec/apps/web/src/components/layout/RouteTransition.tsx`
  > - Wire into `tejospec/apps/web/src/components/layout/Layout.tsx`

- [x] **Enhance LanguageSwitcher micro-interactions**
      Task ID: phase-2-ux-03

  > **Implementation**:
  >
  > - Animations for dropdown open/close and selection
  > - Respect `prefers-reduced-motion`

- [x] **Integrate Premium UX into key pages**
      Task ID: phase-2-ux-04

  > **Implementation**:
  >
  > - Home: wrap hero / key sections
  > - Shop + Product: apply SpotlightCard treatment where appropriate

---

## Verification (after Phase 2)

- [x] Run `pnpm exec nx run @tejo/web:test` (must PASS; 28/28)
- [x] Run `pnpm exec nx run @tejo/web:typecheck` (must PASS)

---

## Phase 3: B2B Wholesale (Canonical)

- [x] **Wholesale services + controller (pricing, partners, validation)**
      Task ID: phase-3-b2b-01

  > **Implementation**:
  >
  > - `tejospec/apps/api/src/modules/wholesale/*`

- [x] **Wholesale verification scripts (smoke + strict verify)**
      Task ID: phase-3-b2b-02

  > **Implementation**:
  >
  > - `tejospec/scripts/verify-wholesale*.mjs`

- [x] **Seed wholesale tiers/pricing/discounts**
      Task ID: phase-3-b2b-03

  > **Implementation**:
  >
  > - `tejospec/apps/api/prisma/seed.ts` (`seedWholesalePricing`)

- [x] **Email templates + invoice generation**
      Task ID: phase-3-b2b-04

  > **Implementation**:
  >
  > - `tejospec/templates/emails/partner-*.hbs`
  > - `tejospec/apps/api/src/modules/wholesale/invoice-generator.service.ts`

---

## Phase 4: Advanced Features (Next)

- [x] **Loyalty Program (models + API + dashboard)**
      Task ID: phase-4-loyalty-01

- [x] **Blog & Content Hub (CMS + pages + SEO)**
      Task ID: phase-4-blog-01

- [x] **Search & Recommendations (API + UI integration)**
      Task ID: phase-4-search-01

- [x] **Subscriptions (billing + management portal)**
      Task ID: phase-4-subscriptions-01

- [x] **Hardening: restrict admin-only endpoints**
      Task ID: phase-4-hardening-01

  > **Implementation**:
  >
  > - Added shared role-based authorization utilities:
  >   - `tejospec/apps/api/src/modules/auth/decorators/roles.decorator.ts`
  >   - `tejospec/apps/api/src/modules/auth/guards/roles.guard.ts`
  > - Migrated admin-only endpoints to use `@UseGuards(JwtAuthGuard, RolesGuard)` + `@Roles(Role.MANAGER, Role.ADMIN, Role.SUPER_ADMIN)`:
  >   - `tejospec/apps/api/src/modules/loyalty/loyalty.controller.ts` (`/loyalty/admin/adjust`)
  >   - `tejospec/apps/api/src/modules/blog/blog.controller.ts` (create/update/delete/publish + admin get by ID)
  >   - `tejospec/apps/api/src/modules/wholesale-catalog/wholesale-catalog.controller.ts` (list + update status)
  >   - `tejospec/apps/api/src/modules/users/users.controller.ts` (create/list/delete)
  >   - `tejospec/apps/api/src/modules/search/search.controller.ts` (`/search/reindex`)

---

## Verification (Phase 4 Readiness)

- [x] Run `pnpm exec nx run @tejo/web:lint` (must PASS)
- [x] Run `pnpm exec nx run @tejo/api:lint` (must PASS)
- [x] Run `pnpm exec nx run @tejo/web:test` (must PASS)
- [x] Run `pnpm exec nx run @tejo/web:build` (must PASS)
- [x] Run `pnpm exec nx run @tejo/api:build` (must PASS)
