---
id: continue-20260104-011-phase4-all-tracks
timestamp: 2026-01-04T00:00:00Z
executed: false
originalPrompt: "all of it"
---

# Improved Prompt

## Objective

Implement Phase 4 across all tracks (Loyalty, Blog/Content Hub, Search/Recommendations, Subscriptions) in the canonical Nx workspace under `tejospec/`, delivering MVP-quality end-to-end slices while keeping lint/build/test green.

## Scope

- Loyalty Program: customer loyalty account + points history + basic UI.
- Blog & Content Hub: public blog listing + post pages with SEO basics; admin flows only as needed.
- Search & Recommendations: product search UI + API; recommendations can start as a simple “related products” placeholder if ML service is not yet present.
- Subscriptions: subscription plans + customer portal MVP; Stripe integration should be scaffolded behind environment variables and safe fallbacks.

## Constraints

- Work only in canonical code: `tejospec/apps/api/**`, `tejospec/apps/web/**`, and shared packages as needed.
- Avoid changes under legacy folders (`tejospec/backend/**`, `tejospec/frontend/**`, `tejospec/archive/**`).
- Prefer small, reviewable increments; validate after each track slice.

## Acceptance Criteria

- Each track has at least one working end-to-end slice (API + web where applicable).
- Authorization stays consistent (admin-only routes use role guards).
- Nx verification passes for impacted targets (lint + build, and tests where available).
