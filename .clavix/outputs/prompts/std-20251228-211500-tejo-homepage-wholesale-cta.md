---
id: std-20251228-211500-tejo-homepage-wholesale-cta
timestamp: 2025-12-28T21:15:00Z
executed: false
originalPrompt: "Homepage on localhost:3003 looks unchanged; make wholesale funnel obvious on / and confirm routes work."
---

# Improved Prompt

## Objective

Make the wholesale funnel **visibly discoverable on the homepage** so a user visiting `http://localhost:3003/` immediately sees that something changed.

## Scope

- Update the homepage hero CTA area to include a persistent link to the wholesale funnel:
  - `GET /wholesale`
  - `GET /wholesale/catalog-request`

## Constraints

- Keep changes minimal and consistent with existing Tailwind styling.
- Do not change routing or business logic.
- Avoid translation churn; use short, clear label text.

## Acceptance Criteria

- Visiting `http://localhost:3003/` shows a clear CTA/button for **B2B / Wholesale**.
- Clicking it navigates to `/wholesale`.
- `/wholesale/catalog-request` still renders and submits successfully.
