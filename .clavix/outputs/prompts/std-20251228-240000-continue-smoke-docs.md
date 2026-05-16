---
id: std-20251228-240000-continue-smoke-docs
timestamp: 2025-12-28T23:59:59Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Make the wholesale pipeline easier to verify and operate by improving the smoke validation ergonomics and documenting the canonical “how to run it” flows (proxy mode + admin mode), without leaking secrets.

## Scope

- Update the smoke script to support a strict/fail-fast mode vs the current “best-effort/skip” behavior.
- Document how to run the smoke script in:
  - proxy mode (via Next `/api/*`)
  - direct API mode
  - optional admin verification (token or login)
- Add a short troubleshooting section for common failures (e.g., 404 due to backend not mapping new routes until restart).

## Constraints

- Do not print or hardcode credentials into docs; reference seed file location instead.
- Keep changes minimal and repo-style-consistent.
- Prefer Nx-based commands when adding any runnable examples.

## Acceptance Criteria

- `tejospec/scripts/smoke-wholesale.mjs` supports a strict mode env flag (documented) that controls whether missing optional dependencies (e.g., DB verification) should fail the run or be skipped.
- A doc exists in `tejospec/docs/` (or README update) showing the recommended smoke commands and env vars.
- Running `node --check` on the smoke script succeeds.
