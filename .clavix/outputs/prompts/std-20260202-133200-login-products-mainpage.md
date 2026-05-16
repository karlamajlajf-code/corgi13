---
id: std-20260202-133200-login-products-mainpage
timestamp: 2026-02-02T13:32:00Z
executed: false
originalPrompt: "login isnt working\nproducts pages are not loaded, fullfiled and completed etc\nMainPage completelly messed up!"
---

# Improved Prompt

## Objective

Diagnose and fix the reported UI regressions: login flow failing, product pages not loading, and the main page layout/content being broken.

## Scope

- Reproduce the issues in the target environment and capture browser console/network errors.
- Inspect the relevant web app routes and components for login, products listing/detail, and the main page.
- Implement the minimal code changes needed to restore functionality and layout.
- Add or update focused E2E or unit tests only if necessary to prevent regression.

## Constraints

- Prefer minimal, targeted fixes in `apps/web` (and shared API/client code if needed).
- Avoid broad refactors; keep changes tightly scoped to the failing flows.
- Run the narrowest relevant verification (lint/test/e2e) once fixes are in place.

## Acceptance Criteria

- Login succeeds with valid credentials and presents expected post-login UI.
- Product listing and detail pages load and render expected content.
- Main page renders correctly without layout breakage or missing content.
- Any new errors in console/network are resolved, and verification checks pass.
