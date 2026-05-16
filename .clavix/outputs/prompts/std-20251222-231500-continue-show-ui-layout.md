---
id: std-20251222-231500-continue-show-ui-layout
timestamp: 2025-12-22T23:15:00Z
executed: true
originalPrompt: "Continue: get the project running, show the web UI again (style/layout), and verify API + web are reachable."
---

# Improved Prompt

## Objective

Ensure the dev environment is running and visually consistent by restoring the global web `Layout` (Header/Footer), then verify both API and web are reachable.

## Scope

- Confirm `apps/web` serves on `http://localhost:3003`.
- Confirm `apps/api` serves on `http://localhost:8003` and health endpoint is OK.
- Ensure the global `Layout` wraps all pages via the Next.js Pages Router `_app.tsx`.

## Constraints

- Do not copy assets or text directly from third-party sites.
- Keep changes minimal and focused on restoring existing layout/styling.

## Acceptance Criteria

- Visiting `http://localhost:3003` renders the global header/top bar and footer on the home page.
- API health endpoint returns `{ "status": "ok" }`.
- `apps/web/src/pages/_app.tsx` wraps the rendered page in `Layout`.
