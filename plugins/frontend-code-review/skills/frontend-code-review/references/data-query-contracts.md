# Data, Query, And Contract Rules

Use these rules for API contracts, query/mutation libraries, auth/SSR boundaries, URL state, and client persistence.

## API Contracts

Flag:

- New legacy service/helper wrappers around generated query or mutation options when a generated client exists.
- Continuing to use deprecated contract operations when a ready generated contract exists.
- Assuming a generated file means an operation is ready without checking deprecated markers, schema shape, and the actual UI consumer.
- Re-declaring API DTOs in components.
- Adding compatibility layers instead of migrating the pointed line and deleting the old layer.

Use the project's generated or shared API types as the shape source of truth. Follow existing input-shape conventions.

## Queries

Flag:

- `enabled` used to hide missing required input instead of a skip-token or equivalent guard.
- Fake fallback IDs or placeholder inputs used to force a query to run.
- Query results copied into local state for rendering.
- Shared query behavior such as invalidation, stale defaults, or retry rules reimplemented at call sites.
- `prefetchQuery` treated as a hard gate or as returning data/errors to the caller.

Use generated or shared `queryOptions()` directly unless a feature hook performs real orchestration.

## Mutations

Flag:

- Deprecated invalidation/reset helpers when the project has a newer pattern.
- `mutateAsync` used without a need for Promise semantics.
- Awaited mutations without error handling.
- Components owning shared cache invalidation that belongs in query defaults.
- Optimistic updates that do not match current list/detail ownership.

Use generated `mutationOptions()` directly when possible. Put shared cache behavior in centralized query defaults.

## SSR, Auth, And Route Boundaries

Flag:

- Request-time auth, setup, role, or tenant decisions moved into static config redirects.
- Dynamic role gates implemented as static path redirects.
- Authorization logic depending on soft `prefetchQuery`.
- Removing a client fallback before server API unavailable behavior is defined.
- Global placeholder query contracts introduced to solve a route-local Suspense issue.
- Branding-sensitive UI reading placeholder defaults without checking pending/placeholder state.

Separate hard gates from soft prefetches. `fetchQuery` can be a server decision boundary; `prefetchQuery` is cache warmup.

## Tenant And Scope Boundaries

Flag:

- Treating tenant/workspace switch as ordinary CRUD invalidation when the app flow performs server switch plus full reload.
- Query keys that omit tenant/workspace identity when the query truly varies by scope and no full reload boundary applies.
- Mixing scope identifiers without tracing the current backend/API contract.

Review tenant/workspace switch as a cache boundary first.

## URL State And Local Storage

Flag:

- Shareable filters, tabs, pagination, selected panels, or search state hidden only in component state.
- One-shot navigation signals modeled as subscribed persistent state.
- Live app state stored in localStorage.
- Direct `window.localStorage` or raw storage calls in app code when a project hook or abstraction exists.
- High-frequency interaction state persisted on every change instead of on commit/settle.

Use URL state for shareable UI state, store/composable state for live UI state, and localStorage only for low-frequency client-only preferences, dismissed notices, and UI defaults.
