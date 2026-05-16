---
id: std-20251228-241000-continue-ci-proxy-mode-robust
timestamp: 2025-12-28T00:10:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Make the GitHub Actions wholesale smoke workflow’s api/proxy switch reliable by avoiding runtime-updated `env` values in `if:` expressions, and document the new manual dispatch usage.

## Scope

- Workflow: Update `.github/workflows/smoke-wholesale.yml` so proxy-mode conditional steps use a step output (via `$GITHUB_OUTPUT`) instead of `if: env.RUN_MODE == 'proxy'`.
- Docs: Update `tejospec/docs/WHOLESALE_CATALOG_REQUEST_SMOKE_TEST.md` to include how to run the workflow manually via `workflow_dispatch` and what “api” vs “proxy” means.

## Constraints

- Keep YAML changes minimal and schema-safe; avoid brittle inline expression hacks in `env:` values.
- Preserve existing behavior:
  - Push/PR runs default to api mode.
  - Manual runs can choose proxy mode and start `frontend`.

## Acceptance Criteria

- No YAML/schema errors for `.github/workflows/smoke-wholesale.yml`.
- Proxy-mode-only steps (wait for web) run when `mode=proxy` in `workflow_dispatch`.
- Docs clearly explain manual dispatch and required secrets/expectations.
