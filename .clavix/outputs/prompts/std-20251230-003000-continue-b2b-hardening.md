---
id: std-20251230-003000-continue-b2b-hardening
timestamp: 2025-12-30T00:30:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Continue stabilizing the canonical Tejospec wholesale experience by hardening the default-off B2B route gate so unfinished B2B flows cannot be reached accidentally (even if middleware is bypassed), and make local setup clearer via env templates/docs.

## Scope

1. **Env template clarity**: Add `NEXT_PUBLIC_ENABLE_B2B_WHOLESALE=false` (default) to `tejospec/apps/web/.env.example` with a short comment.

1. **Defense-in-depth gating**: Add `getServerSideProps` redirects for `apps/web/src/pages/auth/signup-b2b.tsx`, `apps/web/src/pages/checkout/b2b.tsx`, and `apps/web/src/pages/wholesale/bulk-upload.tsx` when the flag is off.

1. **Single source of truth for flag parsing**: Refactor `apps/web/src/middleware.ts` to use `isB2BWholesaleEnabled()` from `apps/web/src/lib/featureFlags.ts`.

1. **Docs touch-up (optional, minimal)**: Add a brief note in `tejospec/README.md` under wholesale about the default-off B2B gate and env var.

## Constraints

- Do not delete legacy `tejospec/backend/**` or attempt endpoint migration (that requires an explicit Option A/B decision).
- Keep existing redirect destination and query contract stable.

## Acceptance Criteria

- With the env flag unset/false, requests to `/auth/signup-b2b`, `/checkout/b2b`, and `/wholesale/bulk-upload` always redirect to `/wholesale/catalog-request?blocked=b2b`.
- `pnpm verify:wholesale` passes.
- `pnpm --filter @tejo/web typecheck` passes.
