---
name: frontend-code-review
description: Use when reviewing frontend pull requests, UI implementation, component changes, CSS/layout, accessibility, responsive behavior, design-system usage, or client-side state/event logic.
license: MIT
---

# Frontend Code Review

## Review Stance

Act as a frontend reviewer. Prioritize user-visible bugs, regressions, accessibility failures, broken interactions, layout problems, data/state issues, and missing verification. Avoid broad refactors, taste-only feedback, or commentary that cannot be tied to a concrete risk.

## Workflow

1. Establish scope:
   - Inspect the changed files and nearby code before judging.
   - Identify the framework, routing/state libraries, styling system, design system, test setup, and available scripts from the repo.
   - If a change touches generated files or vendored assets, verify the source of truth before commenting.

2. Review behavior first:
   - Component props, emits/events, state lifetimes, async loading, error/empty states, forms, validation, focus handling, keyboard behavior, navigation, and persistence.
   - Check that loading, disabled, failure, and partial-data states do not create dead ends or misleading UI.
   - Look for stale closures, unhandled promise paths, duplicate side effects, hydration mismatches, and client/server boundary mistakes.

3. Review UI implementation:
   - Layout stability across desktop and mobile, overflow, clipping, text wrapping, z-index, sticky/fixed positioning, scroll containers, and dynamic content length.
   - Styling consistency with existing tokens, components, spacing, typography, icons, and interaction states.
   - Avoid approving UI that only works for the happy-path screenshot.

4. Review accessibility:
   - Semantic elements, labels, names, roles, heading order, focus order, visible focus, keyboard-only use, ARIA validity, touch target size, contrast, reduced motion, and screen-reader-only content.
   - Prefer native HTML controls over custom ARIA widgets unless the custom behavior is justified and complete.

5. Verify appropriately:
   - Run the smallest relevant checks available: typecheck, lint, unit/component tests, and targeted build.
   - For visible UI changes, use the local app or browser when feasible. Inspect at least one desktop and one mobile-sized viewport.
   - If the app cannot run, state the exact blocker and review from static evidence.

## Findings Format

Lead with findings, ordered by severity. Each finding must include:

- Severity: `[P0]` breaks production or security; `[P1]` major user-visible regression; `[P2]` likely bug or accessibility problem; `[P3]` minor maintainability or polish issue.
- File and line reference.
- Concrete scenario: what user action, viewport, data state, or environment exposes the issue.
- Impact and a practical fix direction.

If no issues are found, say that clearly and include residual risks or checks that were not run.

## What Not To Do

- Do not rewrite the implementation unless the user asks for fixes.
- Do not block on subjective style preferences when the code matches local conventions.
- Do not ask for screenshots instead of running the local app when the app is available.
- Do not claim accessibility, responsive behavior, or visual correctness without evidence.
- Do not bury serious findings under summaries or compliments.
