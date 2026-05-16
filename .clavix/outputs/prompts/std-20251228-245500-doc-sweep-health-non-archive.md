---
id: std-20251228-245500-doc-sweep-health-non-archive
timestamp: 2025-12-28T00:55:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Sweep the **non-archived** documentation to eliminate drift around backend health endpoints, standardizing backend-direct checks to `http://localhost:8003/api/v1/health`, while preserving correct Next proxy examples (`/api/health` on the web host).

## Scope

- Search and update markdown files under `tejospec/**/*.md` only.
- Do **not** modify `archive/**` or other archived/legacy folders.

## Rules

- If a doc is describing a **direct call to the Nest backend** (port `8003`), it must use `/api/v1/health`.
- If a doc is describing a **browser/web (Next) proxy call** (port `3003` or web host), `/api/health` is allowed/expected because it rewrites to `/api/v1/health`.
- Avoid blanket replacements; update only lines where the intent is clearly backend-direct health.

## Acceptance Criteria

- No remaining backend-direct health references in `tejospec/**/*.md` that point to `/health` or `/api/health` on `localhost:8003`.
- Intentional proxy-mode references remain (e.g., `http://localhost:3003/api/health`).
- Edited markdown files report no lint/errors via editor diagnostics.

## Verification

- Run a targeted search for `localhost:8003/api/health` and `localhost:8003/health` under `tejospec/**/*.md` and confirm it returns 0 matches.
- Run `get_errors` on modified files.
