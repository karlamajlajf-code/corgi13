---
id: std-20251228-240500-fix-smoke-workflow-yaml
timestamp: 2025-12-28T00:00:00Z
executed: true
originalPrompt: "Fix the GitHub Actions wholesale smoke workflow YAML errors (workflow_dispatch/job indentation) and keep the api vs proxy mode logic working."
---

# Improved Prompt

## Objective

Fix `.github/workflows/smoke-wholesale.yml` so it is valid GitHub Actions YAML and supports manual runs via `workflow_dispatch` with a selectable `mode` (`api` or `proxy`).

## Scope

- Correct YAML indentation/structure so `workflow_dispatch` is properly nested under `on:`.
- Ensure `jobs.smoke-wholesale` contains `runs-on`, `env`, `defaults`, and `steps` correctly nested.
- Preserve the existing run-mode behavior:
  - Default `api` mode for push/PR.
  - `proxy` mode starts the `frontend` service, waits for web, then runs smoke using `WEB_BASE_URL`.

## Constraints

- Avoid brittle GitHub Actions inline expression gymnastics in `env:`; prefer setting values via a step and `$GITHUB_ENV`.
- Keep changes minimal and limited to the workflow file.

## Acceptance Criteria

- YAML validates (no schema/indentation errors).
- Workflow triggers still work for `push`, `pull_request`, and `workflow_dispatch`.
- In `workflow_dispatch` with `mode=proxy`, the workflow starts `frontend` and the “Wait for web” step runs.
- In `workflow_dispatch` with `mode=api` (or push/PR), the workflow does not require `frontend`.
