---
id: std-20251223-always-on-compose
timestamp: 2025-12-23T14:30:00Z
executed: true
originalPrompt: "Ensure frontend/backend stay running via docker compose; frontend health still installing"
---

# Improved Prompt

**Objective:** Harden docker-compose so the frontend and backend stay up by default, reduce repeated pnpm downloads, and give the frontend enough warm-up time to become healthy on restart.

**Scope:**

- Update `tejospec/docker-compose.yml` frontend service to persist pnpm/corepack caches, keep restart policy, and extend the healthcheck start period.
- Prefer offline installs to leverage the persisted store.
- Recreate the frontend container to apply the compose changes and confirm services stay up.
- Verify container status and recent logs to ensure the frontend becomes healthy alongside the backend/db/redis.

**Constraints:**

- Do not change ports or service names.
- Keep using `pnpm dev` for the frontend entrypoint.
- Avoid touching other services (admin, ml, etc.).

**Acceptance Criteria:**

- docker-compose changes applied with persistent pnpm/corepack volumes and longer healthcheck start period.
- Frontend container starts successfully with cached installs available for subsequent restarts.
- `docker compose ps` shows backend/db/redis running and frontend progressing to healthy (no crash loop).
- Latest frontend logs show install progressing or server ready without errors.
