---
id: std-20251221-000002-ci-repo-hygiene
timestamp: 2025-12-21T00:00:02Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Implement the highest-impact, low-risk repo hygiene fixes identified in the audit:

1. Make GitHub Actions CI workflows discoverable at the **repository root**.
2. Prevent accidental secret commits by hardening `.gitignore` rules for `.env*` files.
3. Reduce dependency determinism drift by standardizing on `pnpm` where possible (at least stop `package-lock.json` from lingering in the pnpm workspace).

## Scope

### In scope

- Create repo-root workflows under `.github/workflows/` that run against the actual monorepo root at `tejospec/`.
- Start by implementing a canonical CI workflow based on `tejospec/.github/workflows/tejobeauty.yml`, updated to run with `working-directory: tejospec` and correct cache settings.
- Update `tejospec/.gitignore` to ignore:
  - `.env.local`
  - `.env.*`
  - `.env.*.local`
- If `tejospec/frontend/web/.env.local` or `tejospec/frontend/web/package-lock.json` are tracked, remove them from git index (`git rm --cached`) without printing their contents.
- Fix the markdown formatting error at the end of `tejospec/docs/PROJECT_AUDIT_2025-12-21.md` (unmatched fenced code block).

### Out of scope

- Refactoring CI to fully converge all workflows to `pnpm`/Nx.
- Large changes to app architecture (e.g., removing competing backends/frontends).
- Any secret scanning or key rotation (note as follow-up only).

## Constraints

- Do not open or print the contents of any `.env*` files.
- Prefer minimal, safe changes; avoid broad refactors.
- Keep existing workflows in `tejospec/.github/workflows/` intact for now; do not delete them automatically.

## Acceptance Criteria

- `.github/workflows/tejobeauty.yml` exists at repo root and runs commands from `tejospec/` (via `defaults.run.working-directory: tejospec` and correct `cache-dependency-path`).
- `tejospec/.gitignore` ignores `.env.local` and `.env.*` patterns.
- `git ls-files` does not include `tejospec/frontend/web/.env.local`.
- The audit report markdown renders correctly (no dangling code fence at EOF).

## Suggested Verification

- Confirm the new workflow files exist under `.github/workflows/`.
- Run `git status` and verify no secrets are staged.
- If feasible locally: `pnpm -C tejospec install --frozen-lockfile` (skip if dependencies are huge / slow).
