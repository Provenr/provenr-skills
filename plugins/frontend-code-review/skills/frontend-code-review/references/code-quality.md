# Code Quality Rules

## Scope Control

Flag changes that expand beyond the requested feature or review scope:

- Repo-wide cleanup mixed into a targeted fix.
- Compatibility exports, aliases, shims, or wrapper layers added without an explicit migration requirement.
- Shared abstractions created before there is stable cross-feature reuse.
- Business components moved into generic shared locations without a clear ownership boundary.

## TypeScript

Flag:

- `any` or broad `Record<string, any>` where generated/API types or local domain types exist.
- Re-declared API shapes instead of importing generated or returned types.
- Weak route/query param typing that leaks `string | string[] | undefined` deep into components.
- Runtime wrappers added only to satisfy TypeScript when a narrower type boundary would preserve the existing runtime shape.

Prefer:

- Explicit domain names that match the API contract.
- Type narrowing at route/API boundaries.
- Small conversion helpers colocated with the component that needs them.

## Styling

Flag:

- New CSS modules or ad hoc CSS when the project's utility-first or design-system approach already covers the need.
- Hardcoded magic values for colors, spacing, radius, shadow, z-index, or typography when design tokens or component variants exist.
- `!important` modifiers or important CSS overrides without a narrow, documented reason.
- Manual string concatenation, template strings, array `.join(' ')`, or custom ternaries for conditional or multi-line classes when a `cn()` / `clsx()` utility exists.
- JS conditional class branches for primitive visual states already exposed by headless primitives through `data-*` selectors.
- Incoming `className` placed before default classes in `cn(...)`, preventing call-site overrides.
- Arbitrary z-index or one-off layering fixes on overlays.

Use:

- The project's class-merge utility (`cn`, `clsx`, etc.) consistently.
- Design tokens and utility classes over one-off values.
- Existing component variants before one-off class forks.
- Primitive selectors such as `data-disabled:*`, `data-checked:*`, `data-highlighted:*`, `group-data-*`, `peer-data-*`, and `has-[:focus-visible]` before adding React/Vue state or boolean props solely for styling.

## Imports

Flag:

- Barrel imports from heavy libraries or design-system packages when direct subpath imports are available.
- Cross-feature imports that bypass explicit public API surfaces.
- Direct imports from generated/internal implementation files when a feature contract already exposes the intended surface.

## Copy And i18n

Flag:

- User-facing hardcoded strings when the project uses i18n.
- Added or renamed i18n keys missing from every supported locale for the touched namespace.
- Translation namespace drift.
- Generic button labels like `Continue` where the action is specific.
- Error messages that state only the failure and not the next step.

Use feature-local translation keys by default. Run the project's i18n validation when available.
