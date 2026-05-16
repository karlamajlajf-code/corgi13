---
id: std-20251229-101500-continue-fix-ci-wholesale-workflow
timestamp: 2025-12-29T10:15:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Restore and harden the GitHub Actions wholesale smoke workflow so it is YAML-valid, uses correct Docker Compose service names/ports, and reliably runs in `api`, `proxy`, and `both` modes.

## Scope

- Workflow file: `.github/workflows/smoke-wholesale.yml` (repo root is `tejospec/`).
- Ensure job structure and indentation are correct (`runs-on`, `timeout-minutes`, `env`, `steps`).
- Validate that Docker Compose service names referenced in the workflow exist and that ports/health endpoints used by readiness checks match the compose configuration.
- Keep the existing run-mode behavior:
  - `both` runs `pnpm verify:wholesale:ci`
  - otherwise run `pnpm smoke:wholesale:ci` and set `WEB_BASE_URL` for `proxy`.

## Constraints

- Keep changes minimal and focused; do not refactor unrelated CI.
- Do not remove existing path filters/triggers unless they’re clearly wrong.
- Prefer deterministic installs (`pnpm install --frozen-lockfile`).

## Acceptance Criteria

- The workflow YAML validates (no schema/indentation errors).
- `docker compose up -d db redis backend frontend` references valid service names.
- API readiness check hits the correct URL for the backend health endpoint.
- Web readiness check hits the correct URL for proxy mode.
- Local developer verification can still run via `pnpm verify:wholesale` (non-CI mode) without requiring GitHub secrets.

## Verify

- Validate YAML via editor diagnostics.
- Run `pnpm verify:wholesale` locally (best-effort; if Docker isn’t available, report that clearly).
