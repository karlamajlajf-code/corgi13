---
id: std-20251222-203000-continue-session-report-banners
timestamp: 2025-12-22T20:30:00Z
executed: true
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Reduce onboarding drift by clearly labeling additional top-level session/report markdown documents as historical/legacy artifacts, steering readers to the canonical runbook.

## Scope

- Target only top-level markdown docs under `tejospec/` that can be mistaken for canonical onboarding/run instructions.
- Add a prominent legacy banner near the top that points to `docs/MASTER_PLAN.md` and reiterates canonical local URLs:
  - Web: `http://localhost:3003`
  - API: `http://localhost:8003/api/v1`
- Prefer minimal markdownlint suppression headers for heavily historical documents rather than reformatting entire snapshots.
- Do not change legacy content beyond the banner/guards (no rewriting history).

## Constraints

- Keep canonical workflow: pnpm-only, Nx preferred.
- Do not add or expose secrets.
- Keep changes minimal and consistent.

## Acceptance Criteria

- The selected docs have an unmissable legacy banner pointing to `docs/MASTER_PLAN.md`.
- Markdown diagnostics for edited docs are clean (or intentionally suppressed for historical snapshots).
- `docs/MASTER_PLAN.md` Change Log records the sweep.
