---
id: std-20251229-000600-continue-ci-web-health
timestamp: 2025-12-29T00:06:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Harden the wholesale CI workflow’s readiness checks so `proxy` and `both` modes reliably wait for the Next.js proxy endpoint to be healthy (not just the site root), then re-validate the workflow YAML.

## Scope

- Update `.github/workflows/smoke-wholesale.yml`:
  - In the “Wait for web (proxy mode)” step, poll `http://localhost:3003/api/health` (proxy health) instead of `http://localhost:3003` to avoid false failures due to 404s on `/`.
  - Keep existing behavior for `api` mode (no frontend start/wait).
- Ensure the workflow remains valid YAML and continues to support `api`, `proxy`, and `both` dispatch modes.

## Constraints

- Keep changes minimal and localized to the readiness check.
- Don’t change runtime ports or service names.

## Acceptance Criteria

- The workflow file has no YAML validation errors.
- `proxy` and `both` modes wait on `http://localhost:3003/api/health` and fail with useful logs if it never becomes healthy.

## Verify

- Run editor validation (`get_errors`) on `.github/workflows/smoke-wholesale.yml` and confirm it reports no issues.
