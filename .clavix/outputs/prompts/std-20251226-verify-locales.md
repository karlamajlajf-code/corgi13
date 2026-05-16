---
id: std-20251226-verify-locales
timestamp: 2025-12-26T12:10:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

**Objective:** Verify Next.js i18n routing works on dev (webpack) at port 3003 for `/en`, default `/hr`, and `/de`, with clean typecheck and lint.

**Scope:**

- Start dev server for `@tejo/web` using `pnpm --filter @tejo/web run dev`.
- Open `http://localhost:3003/en`, `http://localhost:3003/hr`, `http://localhost:3003/de` to confirm pages load.
- Run `pnpm nx run @tejo/web:typecheck` and `pnpm nx run @tejo/web:lint` to ensure stability.

**Constraints:**

- Do not modify application code unless issues are found.
- Keep Nx/pnpm workflow consistent with existing workspace.

**Acceptance Criteria:**

- Dev server starts without errors (warnings acceptable if non-blocking).
- Each locale route loads successfully in the browser.
- Typecheck and lint pass with 0 warnings/errors.
