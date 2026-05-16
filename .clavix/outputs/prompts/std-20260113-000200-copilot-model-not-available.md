---
id: std-20260113-000200-copilot-model-not-available
createdAt: 2026-01-13T00:02:00Z
executed: false
originalPrompt: |
  "Sorry, your request failed... Reason: Request Failed: 400 {\"error\":{\"message\":\"Requested model not available for session\"}} zasto ovo dolazi? i rjesi to."
---

# Improved Prompt

## Objective

Diagnose and resolve the GitHub Copilot Chat error:

> `Request Failed: 400 {"error":{"message":"Requested model not available for session"}}`

Provide clear, step-by-step actions the user can take in VS Code to get Copilot Chat working again.

## Scope

- Explain the most common root causes of the error.
- Provide a prioritized troubleshooting checklist.
- Include specific VS Code / Copilot UI actions (model picker, sign-out/in, update extensions, settings reset).
- Cover both Personal and Enterprise/Organization-managed Copilot scenarios.

## Constraints

- Do not assume the selected model is available for all Copilot plans/orgs.
- Avoid repo/code changes unless a local VS Code setting file is clearly responsible.
- Keep steps actionable and minimal; start with fastest fixes.

## Acceptance Criteria

- User can successfully send a message in Copilot Chat after applying the steps.
- If org policy is the blocker, user knows exactly what to ask their admin (which setting/policy and what symptom).
