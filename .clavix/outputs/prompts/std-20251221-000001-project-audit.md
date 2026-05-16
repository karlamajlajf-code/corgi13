---
id: std-20251221-000001-project-audit
timestamp: 2025-12-21T00:00:01Z
executed: true
originalPrompt: "analyze WHOLE COMPLETE project ALL FILES, make a list what needs to be improved, fixed, added, etc etc."
---

# Improved Prompt: Full-Repository Technical Audit (Actionable Backlog)

## Objective

Perform a repository-wide technical audit of the entire workspace (monorepo) and produce a prioritized, actionable backlog of what should be improved, fixed, added, refactored, or removed.

## Scope

- Audit all major areas (frontend, backend, shared packages, tooling, CI/test setup, infra scripts, docs).
- Use high-signal automated checks and repo-wide searches rather than attempting to read every single file line-by-line.

## Constraints

- Do **not** implement fixes in this pass (no feature work/refactors) unless explicitly requested.
- Be honest about confidence/coverage. If something can’t be confirmed without running a check, say so and specify the check.
- Avoid changing repository behavior; analysis-only is preferred.

## Inputs to Use

- Workspace structure (apps, packages, services, docs)
- TypeScript / ESLint / build configuration
- Existing test reports (e.g., Playwright/Jest outputs)
- VS Code Problems (compiler/linter diagnostics)
- Repo-wide searches for `TODO`, `FIXME`, `HACK`, `@ts-ignore`, `any`, dead-code markers
- Dependency manifests and scripts (package.jsons, nx config)

## Deliverables

Create a markdown report in the repo with:

1. **Executive summary** (top 10 issues)
2. **Prioritized backlog** grouped by severity:
   - 🔴 Critical (security, data loss, broken builds/tests)
   - 🟠 High (significant bugs, unstable architecture, major DX issues)
   - 🟡 Medium (maintainability, perf improvements)
   - ⚪ Low/Nice-to-have
3. For each item:
   - What’s wrong
   - Why it matters (impact)
   - Exact location(s): file paths and relevant areas
   - Suggested fix approach (1–3 steps)
   - Suggested verification (specific command or test)
4. **Quick wins** vs **larger refactors**
5. **Gaps/unknowns** and what to run to confirm

## Acceptance Criteria

- Report is actionable: each item has a concrete location and a clear next step.
- Coverage includes build/test tooling, docs quality, repo hygiene, and runtime risks.
- No hand-wavy advice; prioritize based on impact and effort.
