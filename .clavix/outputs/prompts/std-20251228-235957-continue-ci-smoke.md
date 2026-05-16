---
id: std-20251228-235957-continue-ci-smoke
timestamp: 2025-12-28T23:59:57Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Add a CI-friendly command to run the wholesale smoke test with strict checks and required admin verification, without relying on shell-specific env syntax.

## Scope

- Create a Node wrapper script (cross-platform) that sets default envs:
  - `SMOKE_STRICT=1`
  - `SMOKE_ADMIN_REQUIRED=1`
- Add `pnpm smoke:wholesale:ci` to `tejospec/package.json` to run that wrapper.
- Update the wholesale smoke doc to describe `smoke:wholesale:ci` and the required CI env vars.

## Constraints

- Do not hardcode credentials.
- Defaults should be overridable by explicitly setting env vars in CI.

## Acceptance Criteria

- `pnpm smoke:wholesale:ci` exists and is Windows-safe.
- Wrapper sets defaults only when env vars are unset.
- `node --check` passes on the wrapper and the underlying smoke script.
