---
id: std-20251228-235901-tejo-wholesale-proxy-lint-stabilize
timestamp: 2025-12-28T23:59:01Z
executed: false
originalPrompt: "CONTINUE BUILDING IT AS I SAID BEFORE! (stabilize leads inbox + proxy + verification)"
---

# Improved Prompt

## Objective

Make the wholesale funnel operational end-to-end by ensuring the admin “Leads Inbox” can reliably call the backend in Docker + local dev, and ensure verification is unblocked (API lint passes).

## Scope

1. **Frontend API connectivity (Docker + local):**

   - Ensure browser-side API calls go through Next.js `/api/*` proxy (so the browser never needs to resolve docker-internal hostnames like `backend`).
   - Make `apps/web/next.config.js` rewrites resilient whether `NEXT_PUBLIC_API_URL` is provided as:
     - `http://localhost:8003`
     - `http://localhost:8003/api`
     - `http://localhost:8003/api/v1`
     - `http://backend:8003` (compose)

2. **Verification unblocker:**

   - Fix the current hard ESLint error in API (`no-unused-vars` / unused import) so `@tejo/api:lint` can pass.

3. **No scope creep:**
   - Do not refactor unrelated modules; only touch files needed for proxy reliability + lint unblock.

## Constraints

- Prefer Nx targets for checks (lint/typecheck).
- Preserve existing public routes and the Nest global prefix `/api/v1`.
- Keep changes minimal and backwards-compatible.

## Acceptance Criteria

- Admin page `apps/web/src/pages/admin/wholesale-catalog-requests.tsx` loads successfully in browser when running via Docker (frontend container), and the network calls hit `/api/...` on the same origin.
- Next rewrite correctly forwards `/api/:path*` to the Nest API with the `/api/v1` prefix exactly once (no `/api/v1/api/...`).
- `pnpm nx run @tejo/api:lint` succeeds (no errors).
- `pnpm nx run @tejo/web:lint` remains clean.

## Implementation Notes

- Update `apps/web/src/lib/api/client.ts` so **browser** baseURL uses `/api` (proxy), while **server** can keep using a normalized absolute API URL.
- Add a small normalizer helper in `apps/web/next.config.js` (JS) similar to the one in the API client.
- Remove the unused `Prisma` import from `apps/api/src/modules/loyalty/loyalty.service.ts` (or otherwise resolve the lint error).
