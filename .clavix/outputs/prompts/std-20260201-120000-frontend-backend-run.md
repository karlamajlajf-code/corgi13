---
id: std-20260201-120000-frontend-backend-run
timestamp: 2026-02-01T12:00:00Z
executed: false
originalPrompt: "I want you to first succesfully run and launch frontend, backend, databse base and everything else needed, and leave it running whole time locally so i can see progress and what you are doin. after that continue working you can continue with ✅ Commit this work with a comprehensive message 📊 Start Task 13 (A/B Testing Framework) 🔍 Audit the full tracking pipeline end-to-end 📈 Add integration tests for the tracking system as you asked which one, go 1 by 1, when you finish one, then procced to next one."
---

# Improved Prompt

## Objective

Bring up the full local stack (database, backend API, frontend web app, and any required supporting services) and keep it running while work continues. After the stack is live, proceed through the remaining tasks sequentially: (1) commit current work with a comprehensive message, (2) implement Task 13 (A/B Testing Framework), (3) audit the tracking pipeline end-to-end, and (4) add integration tests for the tracking system.

## Scope

- Start the database via the canonical docker compose configuration.
- Start the API service in dev mode.
- Start the web frontend in dev mode.
- Keep services running in background terminals.
- Then perform the four requested tasks in order.

## Constraints

- Use existing project scripts for dev and database startup.
- Keep the services running throughout subsequent work.
- Provide terminal IDs/log pointers so the user can view progress.
- Do not skip steps; complete each task before starting the next.

## Acceptance Criteria

1. Database, backend, and frontend are launched locally and remain running.
2. A clear report of how to view running logs is provided (terminal IDs or commands).
3. A comprehensive commit is created once stack is running.
4. Task 13 (A/B Testing Framework) implementation is completed.
5. Tracking pipeline is audited end-to-end with findings reported.
6. Integration tests for tracking are added.
