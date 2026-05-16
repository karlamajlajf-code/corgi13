---
id: continue-20260106-013000-wrapper-health-timeout-db-gate
timestamp: 2026-01-06T01:30:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Make the live+Stripe proxy wrapper reliably detect DB-down state and fail fast before running pre-stripe checks (smoke/live or verify/live) and before invoking Stripe smoke.

## Scope

- Update `tejospec/scripts/verify-live-with-stripe-proxy-auto.mjs` so its internal health checks can tolerate real-world `/api/v1/health` latency when DB is down.
- Ensure the existing DB-down fast-fail gate triggers deterministically during `--start-live` runs.

## Constraints

- Keep behavior safe-by-default; no new destructive actions.
- Reuse existing env configuration conventions where possible.
- Avoid changing app runtime behavior; only adjust orchestration script behavior.

## Acceptance Criteria

- With DB down, running the wrapper with `--start-live` exits with code `2` after printing the DB hint, without running `pnpm smoke:live`/`pnpm verify:live` and without invoking `node ./scripts/smoke-stripe.mjs`.
- Wrapper health/probe checks do not regress into false negatives when `/api/v1/health` takes ~4s+.
