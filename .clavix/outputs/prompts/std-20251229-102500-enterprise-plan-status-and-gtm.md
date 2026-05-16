---
id: std-20251229-102500-enterprise-plan-status-and-gtm
timestamp: 2025-12-29T10:25:00Z
executed: false
originalPrompt: "continue WITH these 2 files, check whats done and what not, whats not done, do it"
---

# Improved Prompt

## Objective

Use the two provided documents as the source-of-truth specs:

1. `std-20251227-120000-tejo-growth-90days.md` (GTM/positioning prompt)
2. the enterprise platform implementation plan

Produce concrete, versioned project docs in the repo that (a) reflect current completion status and (b) fill in missing deliverables where feasible.

## Scope

- Audit repo docs to determine which plan phases are already completed (Phase reports, “\*\_COMPLETE.md”, etc.).
- Create a single status doc summarizing what is done, in-progress, and not started.
- Create the 3 GTM markdown deliverables requested by the growth prompt:
  1. Competitor Playbook
  2. Messaging Pack
  3. 90-Day Execution Plan

## Constraints

- Do not implement the entire 11-week engineering plan in one pass.
- Keep output actionable: checklists, KPIs, copy blocks, and weekly milestones.

## Acceptance Criteria

- New docs are created under `tejospec/docs/` (or a `docs/gtm/` subfolder).
- Status doc clearly maps each enterprise plan phase → status with evidence (which completion report/file supports it).
- GTM docs include reusable copy + an operational weekly plan (Weeks 1–13) with KPIs.
