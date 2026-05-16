---
id: std-20251228-200500-fix-docker-frontend-react
timestamp: 2025-12-28T20:05:00Z
executed: false
originalPrompt: "Fix why http://localhost:3003 is not working in docker compose (frontend unhealthy: Cannot find module react)."
---

# Improved Prompt

## Objective

Fix the Docker Compose `frontend` service so `http://localhost:3003` serves successfully and the container becomes **healthy**.

## Context / Root Cause

The `frontend` container returns 500 and logs show `MODULE_NOT_FOUND` for `react` (e.g. it tries to load `/workspace/apps/web/node_modules/react/index.js`). In the container, `apps/web/node_modules/*` entries are broken symlinks pointing to `/mnt/host/.../node_modules/.pnpm/...`, which does not exist in the container. This suggests `apps/web/node_modules` is currently coming from the host bind mount and contains invalid links for this environment.

## Scope

- Update `tejospec/docker-compose.yml` to prevent host-mounted/broken `apps/web/node_modules` from being used.
- Ensure pnpm installs dependencies in-container in a way that Node can resolve them at runtime.
- Keep changes minimal; do not refactor the app itself.

## Constraints

- Windows host + Docker Desktop.
- Use pnpm workspaces as-is.
- Preserve live-reload via bind mount for source code.

## Implementation Notes

1. In `frontend` service, mount a named Docker volume at `/workspace/apps/web/node_modules` (so node_modules lives on a Linux filesystem inside Docker, not the host bind mount).
2. Keep the existing named volume at `/workspace/node_modules` for workspace-level pnpm store if needed.
3. Recreate the `frontend` container and confirm pnpm install repopulates `apps/web/node_modules` with valid links.

## Acceptance Criteria

- `docker compose ps` shows `frontend` as `healthy`.
- `curl -I http://localhost:3003/` returns a non-500 response (200/307/308 acceptable).
- Inside container: `node -p "require('react/package.json').version"` succeeds when run from `/workspace/apps/web`.

## Verification Steps

- `docker compose up -d --force-recreate frontend`
- `docker compose logs --tail=100 frontend`
- `docker compose exec -T frontend sh -lc "cd /workspace/apps/web && node -p \"require('react/package.json').version\""`
- `curl -I http://localhost:3003/`
