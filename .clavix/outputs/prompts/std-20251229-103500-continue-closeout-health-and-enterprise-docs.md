---
id: std-20251229-103500-continue-closeout-health-and-enterprise-docs
createdAt: 2025-12-29T10:35:00Z
executed: true
source: conversation-summary
---

# Improved Prompt

## Objective

Finish the remaining “continuation” gaps for Tejo Beauty wholesale readiness by:

1. making the enterprise plan reference doc markdownlint-clean, and
2. aligning Docker Compose frontend health readiness with CI’s proxy readiness expectations.

## Scope

- Docs:
  - Fix formatting-only markdown lint issues in `tejospec/docs/enterprise/ENTERPRISE_PLAN_REFERENCE.md`.
- Infra:
  - Update `tejospec/docker-compose.yml` so the `frontend` healthcheck uses `http://localhost:3003/api/health` and `frontend` waits for `backend` to be healthy.

## Constraints

- Keep changes minimal and targeted.
- Do not implement large Phase 4+ enterprise plan features; only close out the identified CI/docs readiness gaps.

## Acceptance Criteria

- `ENTERPRISE_PLAN_REFERENCE.md` has proper blank lines around headings/lists and ends with a trailing newline.
- `docker compose -f tejospec/docker-compose.yml config -q` succeeds.
- Frontend healthcheck matches CI proxy readiness endpoint `/api/health`.
