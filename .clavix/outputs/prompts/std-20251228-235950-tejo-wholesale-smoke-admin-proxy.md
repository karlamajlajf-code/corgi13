---
id: std-20251228-235950-tejo-wholesale-smoke-admin-proxy
timestamp: 2025-12-28T23:59:50Z
executed: false
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Add an end-to-end smoke check that validates the wholesale funnel is operational from lead capture through admin processing, including the Next.js `/api/*` proxy path.

## Scope

- Extend the existing wholesale smoke script to:
  - Create a public catalog request.
  - Authenticate as an admin user (token or email/password).
  - List catalog requests filtered by the created email.
  - Update the request status (e.g., `SENT`) via the admin endpoint.
  - Re-fetch and confirm status change.
- Support two execution modes:
  - Direct API mode (calls `http://localhost:8003/api/v1`).
  - Proxy mode (calls `http://localhost:3003/api/*` to exercise Next rewrites).

## Constraints

- Do not add new dependencies.
- Keep the script optional and environment-variable driven (do not hardcode credentials).
- Keep changes minimal and consistent with existing `tejospec/scripts/*.mjs` patterns.

## Acceptance Criteria

- Running the script with admin credentials successfully verifies:
  - Public `POST /wholesale/catalog-requests` returns `{ success: true, id }`.
  - Admin `GET /wholesale/catalog-requests` returns a list containing that `id`.
  - Admin `PATCH /wholesale/catalog-requests/:id/status` succeeds and the request status updates.
- When `WEB_BASE_URL` is set, the script uses `${WEB_BASE_URL}/api/...` paths (proxy mode).
- `pnpm nx run @tejo/web:lint` stays clean.
