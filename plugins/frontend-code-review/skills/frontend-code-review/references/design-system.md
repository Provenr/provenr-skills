# Design System Rules

Use these rules whenever a review touches a shared design system or component library.

Before finalizing findings, read the current local docs that apply:

- Design system README or `AGENTS.md`.
- Overlay/floating-UI docs when reviewing dialogs, drawers, popovers, tooltips, menus, selects, or comboboxes.
- The primitive source for the component being changed or consumed.

## Package Boundary

Flag in design-system packages:

- Imports from application/business layers.
- Dependencies on routing, i18n, HTTP clients, global state, or business APIs.
- Business-specific component behavior that belongs in the app layer.
- Multiple unrelated primitives in one component folder.

A design-system package should be a primitive layer: headless components + variants + class utilities + design tokens.

## Imports And Exports

Flag:

- Consumer imports from package root when subpath exports are required.
- Missing export entry for a new primitive.
- Internal package imports using workspace subpaths instead of relative paths.
- Exported props using internal-only types that consumers cannot import from the public surface.

## Props And State

Flag:

- Flattened props where related values need a discriminated union, such as `value` / `defaultValue`, `multiple` / `value`, or `clearable` / `onChange`.
- Framework state used only to mirror headless primitive state for class names.
- JavaScript conditional class logic for visual states that the primitive already exposes through `data-*` attributes or CSS variables.
- Controlled props added when uncontrolled DOM state or CSS variables would be enough.
- Thin wrappers that rename headless parts without adding semantics.

Prefer headless primitive `data-*` attributes and CSS variables for visual state: `data-open`, `data-checked`, `data-disabled`, `data-highlighted`, `data-popup-open`, `group-data-*`, `peer-data-*`, `has-[:focus-visible]`, and primitive CSS variables. Use JS conditional classes for product/business state that the primitive does not expose.

## Forms

Flag:

- Form-like UI using unrelated input and button pieces without a submit boundary.
- Text-like fields not composed through the design system's field primitives when those semantics exist.
- Field errors or descriptions rendered without proper label/description/error relationships.
- Checkbox/radio groups missing fieldset/legend semantics when required.

The form wrapper is the submit boundary. Design-system form primitives are not a form state-management framework; business validation and schema-driven behavior belong in the app layer.

## Overlay Contract

Flag:

- Legacy overlay imports in new or modified code when a design-system primitive exists.
- Manual portals around overlay primitives that already handle portalling.
- Call-site `z-*` overrides on overlays.
- Missing stacking-context assumptions when debugging overlay layering.
- Repeated backdrop, z-index, or portal chrome at call sites.
- Tooltip used for infotips, long text, or interactive content.

Follow the project's documented z-index and stacking conventions for overlays.

## Primitive Selection

Flag:

- Tabs used for simple mode/filter/view selection where a segmented control is the semantic primitive.
- Segmented control used where `tablist` / `tabpanel` semantics are required.
- Select used for searchable or free-form input.
- Combobox used for unrestricted search text where no selected option is remembered.
- Autocomplete used for closed-list selection.
- Tooltip or hover preview used for content that must be reachable on touch or by screen readers.

Use:

- Autocomplete for free-form text with optional suggestions.
- Combobox for searchable selected values from a collection.
- Select for closed, scannable option sets.
- Popover for infotips, help text, rich content, or interactions.

## Bad Usage Patterns To Flag

Flag:

- Manually recreating UI behavior or chrome already owned by the design system, such as buttons, inputs, toggle groups, popovers, dropdown menus, alert dialogs, switches, avatars, scroll areas, toasts, borders, focus states, disabled states, or segmented controls.
- Styling a raw headless primitive directly in app code when a design-system primitive exists.
- Wrapping a design-system primitive in a feature component that hides its label, error, disabled, or focus contract.
- Replacing a semantic primitive with a generic `div` plus classes to match a screenshot.
- Using Tooltip because it is visually convenient when the content is actually help text or needs touch access.
- Adding a `z-*` override to make a child popup appear over a parent dialog.
- Adding a new app-level wrapper around Dialog, Drawer, Popover, Select, or Combobox that repeats portal/backdrop/positioner logic.
- Building a form row from loose text and controls instead of the matching field/form primitives.
- Adding component state only to style `data-open`, `data-checked`, `data-disabled`, or highlighted states that the headless primitive already exposes.
- Passing booleans down only so children can toggle classes already expressible with primitive `data-*` selectors.

## Tokens And Styling

Flag:

- Generic color utilities where semantic design tokens exist.
- Hardcoded design values where tokens, component variants, or documented radius mappings exist.
- `!important` modifiers used to fight primitive styles instead of fixing the variant, selector, or component composition.
- Manual class strings that duplicate primitive variants.

Use design tokens and documented radius/color mappings. Use `!important` only for a tightly scoped compatibility override after confirming the primitive API, data attributes, and selector structure cannot express the state.

## Focus Details

Flag focus rings attached to the wrong element. For example, a Slider thumb may focus an internal `input[type=range]`, so the visible thumb wrapper needs `has-[:focus-visible]` rather than direct wrapper `focus-visible`.

## Icons

Flag:

- New inline SVG icon components when the project has a centralized icon pipeline.
- Custom icons consumed outside the project's icon class or component convention.
- Icon assets that lose intrinsic dimensions during generation or import.

Follow the project's icon generation and consumption workflow when one exists.
