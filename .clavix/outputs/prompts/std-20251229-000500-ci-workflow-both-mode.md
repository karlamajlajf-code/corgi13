---
id: std-20251229-000500-ci-workflow-both-mode
timestamp: 2025-12-29T00:05:00Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Finish and validate the wholesale smoke GitHub Actions workflow so it supports three run modes (`api`, `proxy`, `both`) and is syntactically valid YAML.

## Scope

- Fix YAML structural errors in `.github/workflows/smoke-wholesale.yml` (especially around the “Wait for web (proxy mode)” step) so VS Code/GitHub Actions YAML parsing succeeds.
- Keep current behavior for `api` and `proxy` modes.
- Add/confirm `both` mode:
  - Start backend always.
  - Start web (Next.js) only when mode is `proxy` or `both`.
  - Wait for `http://localhost:3003/api/health` only when mode is `proxy` or `both`.
  - Run wholesale verification:
    - `api` → run the direct API smoke mode.
    - `proxy` → run the proxy smoke mode.
    - `both` → run `pnpm verify:wholesale:ci` (which runs both proxy + direct API and requires admin checks).

## Constraints

- Don’t refactor unrelated workflow steps.
- Preserve existing secrets usage; do not hardcode credentials.
- Ensure all step-level keys are correctly indented (e.g., `if`, `env`, `run` belong under the step item `- name:`).

## Acceptance Criteria

- `.github/workflows/smoke-wholesale.yml` has no YAML parse/type errors.
- The “Wait for web (proxy mode)” step is a valid step with correct indentation and either `run:` or `uses:`.
- Workflow logic clearly gates web startup + wait-for-web to `proxy` or `both`.
- The workflow selects the correct verification command for each mode.

## Verify

- Use VS Code validation (`get_errors`) for `.github/workflows/smoke-wholesale.yml` and ensure it returns no errors.
