# Vue Page Architecture

Use these patterns for application route pages and their directly supporting
components, composables, constants, utilities, and local types. Adapt them to the
repository's existing UI system instead of imposing specific component names.

## Classify The Surface First

Choose the smallest page shape that matches the product flow:

1. A table CRUD surface for structured records and row actions.
2. A card/grid list when visual preview is the primary way users identify items.
3. A tabbed surface when several independently queryable panels share one route.
4. A route-reused surface when the same structure varies only by resource kind.
5. A purpose-built workspace, editor, graph, report, or analysis page when CRUD
   framing would obscure the main task.
6. A multi-step form when validation or dependencies require a guided sequence.

Preserve loading, empty, error, pagination, navigation, and destructive-action
semantics even when the visual surface is not a table.

## Ownership Boundaries

- Let the route page own route context, top-level assembly, and the placement of
  page-scoped dialogs or drawers.
- Put reactive state, requests, side effects, navigation actions, and
  confirmation coordination in a page or feature composable when doing so makes
  the page easier to read.
- Keep pure initial-value builders, API-to-form transforms, payload builders,
  status rendering, and stable-key construction in ordinary utility functions.
- Do not create forwarding wrapper components merely to make a route page look
  thin. Extract a component for a stable responsibility, an independent panel,
  a meaningful variant, reuse, or substantial template complexity.
- Use props down and typed events up. Use a model only for genuinely shared
  two-way state, and expose a minimal imperative method only when the parent must
  command an independently owned child workflow.

## Route Reuse

- Reuse the same list, form, or detail page when routes differ only by a stable
  resource discriminator. Pass that discriminator as a route prop instead of
  scattering route-name comparisons through the template.
- Centralize derived titles, API selection, target routes, query defaults, and
  test-id scope in a composable or route-context helper.
- Normalize route params once at the boundary, for example with
  `String(route.params.id || '')`, then validate the result before a request.
  Avoid repeating unchecked type assertions at call sites.
- Preserve the repository's current URLs, aliases, names, menu behavior, and
  route metadata unless changing that contract is part of the task.

## Page Composables

- Name page composables for the surface or stable responsibility, such as
  `useListPage`, `useFormPage`, `useDetailPage`, or `useExamRuntime`; avoid vague
  names such as `useData` and `useCommon`.
- Keep each source of truth in one place. Do not maintain duplicate query or form
  state in both a page and its composable.
- Use pure computed values for derivation. Do not request data, write storage, or
  mutate other state from a computed getter.
- Organize returned values consistently: state and computed values first, then a
  blank line, then fetch/render/navigation/handler functions.
- Prefer an existing list-request or async-state composable when its request,
  pagination, cancellation, and reset semantics fit. Check whether it fetches
  immediately before adding a mount hook; the initial request must run once.
- For manual requests, set loading before the request and restore it in
  `finally`. Handle cancellation and stale responses when repeated requests can
  overlap.

## List Pages

- Keep query state as one complete initial object containing pagination, sort,
  and every UI filter. Reset by rebuilding/replacing that object so old filters
  cannot leak into a new query.
- Reset the page before a search or filter change. Preserve both total-count and
  has-next pagination contracts when an API may return either.
- Treat numeric zero and boolean false as real filter values; do not use generic
  truthiness to decide whether a filter is active.
- Keep UI-only range values in the query model and transform them into transport
  filters at the request boundary. Exclude transport-only fields when deciding
  whether the user has active filters.
- Put business actions in the page toolbar and record actions near each row or
  card. Describe action visibility, disabled state, loading, intent, and handler
  in one typed structure when the project has an action abstraction.
- With tabs, let each panel own its query, list, loading, columns, and pagination.
  Let the parent own the active tab and only the state truly shared across tabs.
  Choose `v-show` when state preservation matters and `v-if` when avoiding the
  initial cost matters more.

## Form And Wizard Pages

- Keep form values and the last successfully saved snapshot separate. Build
  default values, detail-to-form transforms, and submission payloads outside the
  template.
- Clone nested snapshots before resetting or handing mutable drafts to child
  components. After reset, clear validation once the DOM/form state has updated.
- Validate before entering the submitting state. Disable reset and submit while
  conflicting load, reset, upload, or submission work is active, and restore
  flags in `finally`.
- After a successful save, update the saved snapshot before navigating so an
  unsaved-change guard cannot block the success path.
- A leave guard must handle page buttons, browser/router navigation, confirm,
  cancel, successful submit, and unmount without leaving a pending promise or
  allowing the same navigation twice.
- In a wizard, keep active/current/first/last-step state and validation policy in
  the page composable. Validate only visible fields when advancing, allow back
  navigation without unrelated validation, and validate all applicable steps
  before the final submission. Return to the first invalid step on failure.
- Let step components emit intent through stable step keys; they should not
  mutate parent refs, route state, or step indexes directly.

## Detail Pages

- Keep the record, loading state, fetch action, navigation actions, and
  resource-state actions in one page workflow.
- Show a purposeful empty/not-found state after loading completes without a
  record, with a clear way back to the list or previous surface.
- Use status constants or domain types for action visibility instead of display
  strings.
- Split comments, graph views, media, tables, and other independent complex
  regions into local components rather than accumulating every workflow in the
  detail template.

## Confirmed Actions

- Require confirmation for destructive or state-changing actions when the
  product semantics warrant it.
- Snapshot the current record identifier before awaiting the API so a reactive
  selection change cannot redirect the action to another record.
- On failure, keep the dialog and target state available for retry and avoid
  success notifications or refreshes.
- Only after success should the workflow close the dialog, show success, update
  the saved state, and refresh or navigate.

## Review Checklist

- The chosen page shape matches the user task instead of forcing every surface
  into a table CRUD layout.
- Route, page, composable, child-panel, and utility responsibilities are clear.
- Initial fetches run once and every loading flag is restored on failure.
- Empty, error, disabled, unsaved, confirmation, and success paths are present.
- Query reset, numeric-zero filters, pagination variants, and multi-step
  validation behave correctly.
- Existing public components and technical feature boundaries are reused before
  lower-level integrations are copied.
