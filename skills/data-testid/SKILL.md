---
name: data-testid
description: Design, add, migrate, or review stable frontend data-testid attributes and E2E selector contracts. Use whenever a task mentions test IDs, data-testid, page test hooks, Playwright/Cypress selectors, testability of lists/forms/dialogs/tabs, or flaky selectors, even when the user only asks to make a UI easier to test.
---

# Data Test IDs

Use this skill to create durable selector contracts for frontend application
surfaces. Prefer accessible role, label, and visible-text locators for ordinary
user behavior; add test IDs at stable page, component, and interaction boundaries
where semantic locators are ambiguous or intentionally changeable.

**Core constraint:** Encode stable UI responsibility, not runtime data, DOM
position, styling, or user-facing copy.

## Scope

- Apply this skill to route pages, independent panels, search/list/form/detail
  flows, tabs, dialogs, drawers, editors, and primary interactions.
- Include leaf components only when they own a critical or otherwise ambiguous
  interaction. Decorative elements and ordinary field display do not need IDs.
- Treat existing IDs used by E2E tests as a public compatibility contract. Do
  not rename them during unrelated cleanup.

## Workflow

- [ ] Inspect routes, feature/module ownership, component boundaries, existing
      IDs, E2E locators, and the project's naming conventions.
- [ ] Choose one stable kebab-case scope for the page or independent panel.
- [ ] Cover the root, primary regions, major actions, and ambiguous controls
      without annotating every DOM node.
- [ ] Route IDs through component props or a scope contract when shared
      components render the actual interactive node.
- [ ] Search for duplicates and stale names, then run targeted E2E or component
      tests when available.

## Naming

Use a compositional shape:

```text
<scope>-<surface-or-region>-<element-or-action>
```

- Derive `scope` from stable product ownership such as `billing-invoice-list` or
  `app-auth-login`, not a CSS class, translated title, or current file basename.
- Use kebab-case only. Avoid spaces, camelCase, localized text, timestamps,
  random values, database IDs, and array indexes.
- Preserve an established scope when a file moves but the user-facing surface
  remains the same.
- For a route-reused page, derive the scope from the explicit resource/route
  discriminator in one place rather than branching throughout the template.
- Add panel or tab identity when multiple copies can be mounted at once so IDs
  remain unique.

## Coverage By Surface

For list or search surfaces, consider IDs for:

- page/panel root, search controls and actions, each ambiguous filter, result
  table/grid, toolbar, primary business actions, empty state, and pagination;
- row/card action renderer and each action intent such as detail, edit, enable,
  disable, export, or delete.

For forms and wizards, consider IDs for:

- page root, top bar/breadcrumb when tests navigate through them, form, logical
  sections, field containers, reset/submit actions, step overview, current step,
  step actions, and page-scoped dialogs;
- field IDs on the form item by default, with additional input/select/upload IDs
  only when one field contains multiple critical controls.

For detail or special-purpose surfaces, consider IDs for:

- page root, primary content, overview, major sections, top-level actions,
  empty/not-found state, dialogs/drawers, and the main editor, graph, timeline,
  chart group, preview, or workspace region.

These are coverage prompts, not a requirement to add every possible ID.

## Repeated Rows And Cards

- Keep action IDs stable across rows, for example
  `billing-invoice-list-row-delete`; do not append the record ID.
- Locate a record by visible identity, accessible name, or a containing row/card,
  then query the action within that container.
- Do not use array indexes. Reordering, filtering, virtual scrolling, and
  pagination make positional selectors fragile.
- When an accessible locator uniquely identifies the record and action, prefer
  it over adding another data attribute.

## Component Boundaries

- Put the ID on the element that owns the interaction or region, not on an extra
  wrapper created only for testing.
- Let shared components accept a `testId`, `testIdScope`, or focused per-control
  IDs when they render internal interactive elements. Do not duplicate the same
  ID on the component root and its child control.
- Keep shared component defaults deterministic, but require a caller-provided
  scope when multiple instances could otherwise collide.
- Do not copy a shared component's private DOM into a page merely to gain test
  access; extend its explicit testability contract instead.

## Anti-Patterns

- Selecting by generated CSS classes, DOM depth, nth-child, mutable copy, or
  translated text when the test is about a stable product action.
- Adding IDs to every label, icon, cell, and decorative wrapper.
- Embedding record IDs, timestamps, random values, or indexes in selector names.
- Renaming stable IDs without updating and running their consumers.
- Changing user-visible wording solely to make an automated test easier.

## Delivery Check

- Confirm IDs are unique in every simultaneously mounted state.
- Confirm names express stable responsibility and contain no runtime identity.
- Check list, empty, filtered, dialog, tab, and wizard states relevant to the
  changed flow.
- Search E2E tests and component consumers before renaming an existing contract.
- Run the repository's targeted E2E/component tests, lint, and typecheck.
