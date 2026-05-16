---
id: continue-20260105-020000-smoke-stripe-start-web
timestamp: 2026-01-05T02:00:00Z
executed: true
originalPrompt: "continue working"
---

# Improved Prompt

## Objective

Improve the Stripe smoke test ergonomics by adding `--start-web` support so proxy mode (`--proxy`) can automatically start/stop the Next.js dev server.

## Scope

1. Update `tejospec/scripts/smoke-stripe.mjs`:

- Add `--start-web` flag.
- In `--proxy` mode, allow `WEB_BASE_URL` to default to `http://localhost:3003`.
- Implement a web-proxy health poll on `${WEB_BASE_URL}/api/health`.
- When `--start-web` is used and the proxy health is not reachable, start `pnpm dev:web` and wait up to `SMOKE_STARTUP_TIMEOUT_MS`.
- Ensure any started web process is stopped on all exit paths.

1. Keep existing behavior unchanged unless flags are explicitly provided.

## Constraints

- Do not commit secrets.
- Keep the script consistent with existing smoke scripts (Windows-safe shutdown via `taskkill`).

## Acceptance Criteria

- `node tejospec/scripts/smoke-stripe.mjs --help` documents `--start-web` and `WEB_BASE_URL` default behavior in proxy mode.
- `pnpm nx run @tejo/api:lint` remains green.

## Verification

- `node tejospec/scripts/smoke-stripe.mjs --help`
- `cd tejospec && pnpm nx run @tejo/api:lint`
