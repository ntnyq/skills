---
name: element-plus
description: Apply ntnyq's Element Plus conventions when writing, refactoring, or reviewing Vue SFCs that use ElButton, ElDialog, ElInput, ElForm, ElTable, ElTableColumn, or related components. Use whenever Element Plus UI code, table columns, dialogs, form controls, or Chinese prompts such as "Element Plus 规范" and "表格列规范" are involved.
---

# Element Plus

Use this skill for component-level conventions on top of Element Plus. Inspect
the installed Element Plus version and the application's existing wrappers,
icons, form helpers, table utilities, and test contracts before editing.

**Core constraint:** Preserve product behavior and existing public component
contracts; these conventions do not authorize replacing project wrappers or
changing API values for display convenience.

## Workflow

- [ ] Inspect the Element Plus version, nearby components, global defaults,
      wrappers, icon system, UnoCSS/CSS setup, and tests.
- [ ] Identify each control's state owner, validation contract, loading/error
      states, and accessibility requirements.
- [ ] Apply the smallest convention-preserving change without duplicating a
      project wrapper or rewriting unrelated columns.
- [ ] Run lint, Vue typecheck, targeted tests, and browser inspection for changed
      visual or interactive behavior.

## Buttons

- Give a text button an explicit `type` or bound `:type`; use `type="default"`
  when no semantic variant applies.
- When a button uses the project's Font Awesome convention with a leading icon
  and visible text, use `mr-1.5` between icon and text and wrap the text in a
  `span`. Preserve a different icon system's established spacing component or
  class instead of forcing Font Awesome markup.
- Keep icon-only buttons accessible with an explicit label or tooltip and do not
  apply text-button wrapping rules to them.
- Bind loading and disabled states to the real in-flight action so duplicate
  submissions are impossible.

```vue
<ElButton type="default">
  <i class="fas fa-check mr-1.5" />
  <span>Confirm</span>
</ElButton>
```

## Dialogs

- In this house style, direct `ElDialog` usage opts into `append-to-body` and
  `align-center`. When forwarding attributes with `v-bind`, place these fixed
  props after the spread so forwarded values cannot override them accidentally.
- Verify teleport implications for scoped styles, focus, test mounting, and
  nested overlays before changing an existing dialog.
- Use `defineModel<boolean>('visible', ...)` when the parent owns visibility.
  Do not wrap that model in another toggle state.
- For local visibility, prefer the project's VueUse `useToggle` convention when
  VueUse is already available. Use `isVisible` for one surface and a specific
  `isXxxVisible`/`setIsXxxVisible` pair when several overlays coexist.
- Keep a failed async confirmation open for retry. Close, notify success, and
  refresh only after the action succeeds.

```vue
<ElDialog v-bind="dialogProps" append-to-body align-center>
  <!-- content -->
</ElDialog>
```

## Inputs And Forms

- Give textarea inputs an explicit row count and `resize="none"` unless manual
  resizing is part of the requested UX. Do not add a meaningless `clearable`
  prop to a textarea.
- Keep `ElForm` model, rules, and the form ref explicit. Validate before entering
  submission state and restore submission/loading flags in `finally`.
- Make visible required/optional/unique guidance agree with the actual rules,
  length limits, accepted file types, and server contract.
- Keep complex payload construction, deep cloning, and API field conversion out
  of the template.

## Table Columns

Every ordinary business column should declare:

- a `label` and the original data `prop`;
- an explicit `align` based on content semantics; and
- either a numeric bound `:width` or `:min-width`.

Selection, index, expand, and synthetic action columns may lack a data `prop`,
but they still need a useful label when visible, alignment, and an intentional
width.

- Use `min-width` for variable-length names, titles, descriptions, and accounts;
  align these left in most tables.
- Use fixed `width` for status, time, numeric, enum, media, and action columns;
  align these center unless the product meaning suggests otherwise.
- Bind numeric widths (`:width="120"`, `:min-width="180"`) rather than passing
  pixel or percentage strings.
- Treat 90 for indexes, 120 for short statuses, 140–160 for short categories,
  200 for date-times, and 100 for compact action menus as starting points, not
  universal requirements.
- Do not add `show-overflow-tooltip` by default. Prefer an appropriate minimum
  width, a summary, a detail route, or a dedicated preview for long content.
- For server sorting, use `sortable="custom"` and keep `prop` equal to the raw
  API sort field even when the cell renders a formatted companion value.
- Keep status values in their original numeric/union form and compare them with
  constants. Numeric zero is a valid status and filter value.
- Prefer API-provided formatted time fields or one shared formatter; do not
  scatter date-format strings through table templates.
- Keep row-action visibility and disabled/loading state explicit. Destructive
  actions still require the application's confirmation flow.

```vue
<ElTableColumn :min-width="180" prop="name" label="Name" align="left" />

<ElTableColumn
  :width="200"
  prop="updatedAt"
  label="Updated at"
  align="center"
  sortable="custom"
/>
```

## Delivery Check

- Confirm forwarded attributes do not override fixed dialog behavior.
- Confirm dialog focus, teleport styling, cancellation, failure, and success
  paths in the browser.
- Confirm each changed column has meaningful data semantics, alignment, width,
  sorting, and empty-value rendering.
- Run `pnpm lint`, `pnpm typecheck` or `pnpm vue-tsc --noEmit`, targeted tests,
  and visual inspection of affected tables/forms/dialogs.
