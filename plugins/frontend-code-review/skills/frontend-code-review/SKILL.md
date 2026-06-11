---
name: frontend-code-review
description: Review frontend code for correctness, accessibility, component design, design-system usage, data/query boundaries, performance, and tests. Trigger for `.tsx`, `.ts`, `.vue`, `.js`, UI, React, Vue, Next.js, Nuxt, pending-change, or focused frontend review requests.
---

# Frontend Code Review

## When To Use

Use this skill when the user asks to review, audit, analyze, or sanity-check frontend code or frontend-adjacent TypeScript/JavaScript files.

Supported modes:

- **Pending-change review**: inspect staged and working-tree changes.
- **File-focused review**: inspect explicitly named files or paths.
- **Diff/snippet review**: review pasted diffs or snippets using best-effort references.

Do not use this skill for backend-only code; use a backend review skill instead.

## Required Context

Before reviewing, read the relevant local contracts when they exist:

- Project `AGENTS.md`, `ClAUDE.md`, or frontend README for workflow, design tokens, state patterns, and test conventions.
- Design system or component library docs when code uses or changes shared UI primitives.
- Overlay/floating-UI docs when reviewing dialogs, drawers, popovers, tooltips, menus, selects, or comboboxes.
- `code-behavioral-guidelines` for scope control and focused, verifiable changes.

For any UI, UX, or accessibility review, fetch the latest Web Interface Guidelines before finalizing findings. Treat them as a required baseline, not the complete source of accessibility truth:

```text
https://raw.githubusercontent.com/vercel-labs/web-interface-guidelines/main/command.md
```

If the review depends on a current framework, SDK, browser API, or accessibility behavior and local code does not settle it, check the current official docs first. For browser compatibility, deprecation, or behavior-sensitive frontend APIs, verify MDN or the relevant standard.

## Rule Packs

Apply every relevant rule pack:

- [references/accessibility-ui.md](references/accessibility-ui.md) — accessibility, semantic HTML, focus, forms, keyboard, disabled states, copy, and long-content behavior.
- [references/design-system.md](references/design-system.md) — design-system primitive usage, overlays, forms, tokens, and component boundaries.
- [references/component-architecture.md](references/component-architecture.md) — component ownership, props, state, effects, exports, wrappers, and feature organization.
- [references/data-query-contracts.md](references/data-query-contracts.md) — API contracts, query/mutation patterns, auth/SSR boundaries, URL and client storage state.
- [references/performance.md](references/performance.md) — React/Vue/SSR performance review rules scoped to real risk.
- [references/testing.md](references/testing.md) — frontend test review rules.
- [references/code-quality.md](references/code-quality.md) — general TypeScript, styling, naming, and maintainability rules.

Skip a rule pack when the change clearly does not touch that area. Do not invent project-specific rules that contradict local docs.

## Review Process

1. Identify the review scope. For pending changes, inspect `git diff --stat`, `git diff`, and staged diff if relevant. For file-focused reviews, stay within the named files unless a referenced owner/contract must be read.
2. Read code around the changed lines and the owning module. Do not review isolated snippets when nearby ownership, labels, query inputs, or overlay structure decide correctness.
3. Check user-visible regressions first: accessibility, broken interaction, auth/permission leaks, query/hydration errors, data loss, navigation mistakes, and impossible states.
4. Then check maintainability and performance: ownership, effects, wrappers, memoization, bundle/waterfall risks, tests, and design-system drift.
5. Report only actionable findings. Do not list speculative risks, style preferences, or broad refactors unless they are directly tied to a reproducible issue in scope.

## Severity

- **P0**: security/privacy/auth leak, data loss, production crash, inaccessible critical flow, or broken primary workflow.
- **P1**: user-visible regression, hydration/SSR failure, invalid API/query contract, broken keyboard/focus behavior, or serious design-system/a11y violation.
- **P2**: maintainability or performance issue likely to cause bugs, duplicated state, incorrect ownership, missing tests for risky behavior, or non-critical a11y issue.
- **P3**: minor cleanup with clear value. Omit unless the user asked for a thorough audit.

## Output Format

Lead with findings, ordered by severity. Use this structure:

```markdown
## Findings

- [P1] Short issue title
  File: `path/to/file.tsx:123`
  Why it matters and how to reproduce or reason about it.
  Suggested fix: concrete fix direction.

## Open Questions

- Question or assumption, if any.

## Summary

Brief secondary context. Mention tests not run or residual risk.
```

Rules:

- If there are no findings, say `No issues found.` and mention any test gaps or residual risk.
- Always include file and line when available.
- Keep findings concrete and reproducible.
- Do not include praise sections by default.
- Do not ask to apply fixes unless the user explicitly wants review plus implementation.
