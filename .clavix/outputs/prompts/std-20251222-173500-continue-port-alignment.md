---
id: std-20251222-173500-continue-port-alignment
timestamp: 2025-12-22T17:35:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Reduce local-run friction by eliminating stale hardcoded local URLs/ports (`localhost:3000`, `localhost:3001`) across _active_ tejospec tooling, so that scripts, Playwright defaults, and web API client fallbacks consistently use the canonical dev ports:

- Web: `http://localhost:3003`
- API: `http://localhost:8003/api/v1`

## Scope (what to change)

1. Update **active scripts** under `tejospec/scripts/` that default to `localhost:3000` to use `localhost:3003`.
2. Update **Playwright config** defaults (`tejospec/playwright.config.ts`) to use `localhost:3003`.
3. Update the **web API client fallback** (`tejospec/apps/web/src/lib/api/client.ts`) to default to `http://localhost:8003/api/v1` instead of the legacy `http://localhost:3001/api`.
4. Update **non-secret env templates** that still advertise `FRONTEND_ORIGIN=http://localhost:3000` (e.g., `tejospec/.env.example`, `tejospec/backend/.env.example`) to the canonical `http://localhost:3003`.
5. Update **primary onboarding docs** that are likely still read (at minimum `tejospec/README.md` and `tejospec/QUICK_REFERENCE_NEXT_STEPS.md`) to reference `localhost:3003` for the storefront.

## Non-goals / Constraints

- Do not migrate Next.js routing or change architecture.
- Do not remove legacy ports from allowlists if it risks breaking old flows; prefer additive compatibility where needed.
- Do not modify archived docs/reports or large historical artifacts.
- Do not print env contents or secrets.

## Acceptance Criteria

- No active runtime script defaults to `localhost:3000` for the storefront.
- Playwright default `baseURL` is `http://localhost:3003`.
- Web API client default points to `http://localhost:8003/api/v1` when `NEXT_PUBLIC_API_URL` is not set.
- Canonical docs (README + quick start) point to `http://localhost:3003`.
- A focused verification run passes (at least `nx` build/typecheck for the web app, or equivalent minimal checks for touched areas).
