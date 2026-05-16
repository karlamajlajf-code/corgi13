---
id: std-20260109-173000-tejospec-restore-en-common-json
timestamp: 2026-01-09T17:30:00Z
executed: false
originalPrompt: "Continue repo-wide Croatian-first i18n/test stabilization; unblock verification by repairing tejospec/apps/web/public/locales/en/common.json JSON corruption, and re-verify locales + E2E specs."
---

# Improved Prompt

## Objective

Repair the **broken English locale dictionary** (`tejospec/apps/web/public/locales/en/common.json`) so it is **valid JSON** and matches the key structure expected by the Next.js web app (`next-i18next`) without reintroducing German-coupled UI/test logic. Then run the narrowest verification checks to confirm the locale files and the updated Playwright specs are consistent.

## Scope

1. **Fix `en/common.json` structural corruption**:
   - Ensure the file parses with `JSON.parse`.
   - Ensure `nav`, `hero`, `common`, and `auth` top-level sections exist and follow the same schema as `hr/common.json`.
   - Move/rewrap auth page dictionaries so they live under `auth.*` (e.g., `auth.loginPage.*`, `auth.registerPage.*`, `auth.forgotPage.*`, `auth.resetPage.*`).
   - Ensure `nav` contains actual navigation labels (Home/Shop/About/Contact/Blog/Cart/Account/Login/Logout/Register/Wishlist) and does **not** contain auth page dictionaries.
2. **Locale hygiene**:
   - Confirm `rewardsPage` exists only once in both `hr/common.json` and `en/common.json`.
   - Confirm no duplicate keys that would silently override critical auth strings.
3. **Keep the existing E2E changes intact** (German strings removed; stable selectors used).

## Constraints

- **Croatian is the primary UX language**: do not reintroduce German strings or German-coupled assertions.
- Prefer **minimal, localized edits** (repair the corrupted section rather than rewriting unrelated dictionary areas).
- Do not change runtime i18n behavior; this is a dictionary integrity + test stability fix.
- Prefer Nx workflows when running repo tasks (per `tejospec/AGENTS.md`).

## Acceptance Criteria

- `tejospec/apps/web/public/locales/en/common.json` parses successfully with `JSON.parse`.
- `tejospec/apps/web/public/locales/hr/common.json` still parses successfully.
- `auth.*` keys referenced by auth pages exist in both locales:
  - `auth.loginPage.*`, `auth.registerPage.*`, `auth.forgotPage.*`, `auth.resetPage.*`, `auth.passwordMismatch`.
- `rewardsPage` appears **exactly once** per locale file.
- A quick grep confirms the updated Playwright specs do not contain common German UI literals.

## Verification Steps

- Parse both locale JSON files with a Node one-liner.
- Grep `tejospec/tests/e2e/specs/*.spec.ts` for German literals used previously.
- (Optional if feasible) run the narrowest Nx task that validates the web app without requiring external services.
