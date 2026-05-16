---
id: std-20260109-201500-tejospec-dedupe-passwordMismatch
project: tejospec
timestamp: 2026-01-09T20:15:00Z
executed: true
executedAt: 2026-01-09T20:18:00Z
originalPrompt: "continue"
---

# Improved Prompt

## Objective

Remove remaining duplicate translation keys that can silently override values and cause inconsistent UI behavior—specifically the duplicated `passwordMismatch` key in both Croatian and English locale dictionaries.

## Scope

Update these files:

- `tejospec/apps/web/public/locales/hr/common.json`
- `tejospec/apps/web/public/locales/en/common.json`

## Requirements / Constraints

- Croatian remains the primary language; do not introduce German-coupled strings.
- Keep `auth.passwordMismatch` as the canonical key (it is already referenced by the app code and tests).
- Remove the duplicate `errors.passwordMismatch` entry (there are no code references to `errors.passwordMismatch`).
- Do not change other keys/values beyond what is required to remove the duplicate.

## Acceptance Criteria

- Each of the above JSON files contains **exactly one** `"passwordMismatch"` key.
- The remaining key is `auth.passwordMismatch` with the existing translation value.
- Both JSON files parse successfully (`JSON.parse`) with no syntax errors.

## Verification

- Parse-check the locale JSON files.
- Optionally run `nx run @tejo/web:lint` (or the repo’s preferred equivalent) to ensure no incidental issues.
