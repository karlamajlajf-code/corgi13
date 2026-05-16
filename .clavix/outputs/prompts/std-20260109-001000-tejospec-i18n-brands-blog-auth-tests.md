---
id: std-20260109-001000-tejospec-i18n-brands-blog-auth-tests
timestamp: 2026-01-09T00:10:00Z
executed: true
originalPrompt: "CONTINUE WORKING!"
---

# Improved Prompt

## Objective

Continue the repo-wide i18n cleanup with Croatian as the primary language by removing remaining hard-coded German UI strings from the Next.js web app, ensuring all user-facing text uses `next-i18next` keys with `hr` + `en` dictionary parity.

## Scope

- Migrate remaining German UI strings to `next-i18next` keys in:
  - `tejospec/apps/web/src/pages/brands.tsx`
  - `tejospec/apps/web/src/pages/blog/[slug].tsx`
- Replace any `alert(...)` user messages with translated strings (keep `alert` if no toast system is available).
- Update or fix related web tests that assert German UI text so they match the new translated UI (Croatian-first).
- Add all new keys to both dictionaries:
  - `tejospec/apps/web/public/locales/hr/common.json`
  - `tejospec/apps/web/public/locales/en/common.json`

## Constraints

- Prefer Nx targets for checks: `pnpm nx run ...`
- Avoid raw i18n key rendering: ensure required namespaces are preloaded (use `getCommonTranslations(locale)` in page `getStaticProps` / `getServerSideProps` patterns).
- Keep existing business logic intact; only refactor structures as needed to store translation keys instead of localized strings.

## Acceptance Criteria

- No hard-coded German UI strings remain in the migrated pages/components.
- `pnpm nx run @tejo/web:lint` passes with **no warnings**.
- `pnpm nx run @tejo/web:typecheck` passes.
- Relevant `@tejo/web` tests (if executed) pass after updating assertions.
