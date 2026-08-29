---
name: vue-sfc-style
description: Apply ntnyq's Vue single-file component coding standards. Use for writing, refactoring, or reviewing .vue files, props/emits/models/slots, composables, template structure, UI state, or Chinese prompts such as "vue-sfc 编程规范", "Vue 组件规范", "SFC 风格", or "重构 Vue 组件".
---

# Vue SFC Style

Use this skill for `.vue` component work. Prefer readable component contracts, typed Composition API, and templates that make user flows clear.

**Core constraint:** Keep each component's public contract, source state, derived
state, side effects, and user-visible states explicit and easy to scan.

## Scope

- Apply this skill to individual `.vue` files, component contracts, reactivity,
  templates, slots, accessibility, and component-local styling decisions.
- Keep application routing, feature architecture, stores, and package publication
  outside this skill unless they directly change the component contract.

## Workflow

- [ ] Inspect the component's callers, nearby SFCs, Vue version, UI system, and
      configured formatter and linter.
- [ ] Define props, emits, models, slots, exposed APIs, state ownership, derived
      values, and side effects before reshaping the template.
- [ ] Implement one focused component using the repository's Composition API and
      styling conventions.
- [ ] Review template states, accessibility, reactivity choices, and component
      boundaries, then run the delivery checks.

## Component Shape

- Prefer `<script setup>` with TypeScript. Let the configured formatter or linter
  determine the order of the `setup` and `lang="ts"` attributes.
- Order blocks as script, template, then style when styles are needed.
- Keep component contracts near the top: imports, props, emits, model, slots, refs/state, computed values, functions, watchers/lifecycle, expose.
- Use `defineProps<T>()`, `withDefaults`, `defineEmits<T>()`, `defineModel<T>()`, and `defineExpose()` with explicit public types.
- Use `useTemplateRef()` for template refs when available in the project.
- Keep a component focused. Extract child components only when it reduces template noise, isolates a meaningful UI unit, or enables reuse.

## Props, Emits, Models

- Type props and emits with object/tuple syntax.
- Keep small local prop shapes inline in `defineProps<{ ... }>()`.
- When a component has three or more props, prefer an `interface Props` above
  `defineProps<Props>()` so the contract remains easy to scan. Give each prop a
  concise multiline JSDoc comment that explains its domain meaning.
- Name a reusable local component props type `Props`; do not include the component
  name, such as `AppProps`. Use `XxxProps` only when exporting the type as part of
  a public API.
- Spell identifier abbreviations as ordinary words in prop and named-model
  names: use `userId` and `selectedUserIds`, not `userID` or
  `selectedUserIDs`. This also produces natural kebab-case template attributes.
- Declare event tuples directly in `defineEmits<{ ... }>()`; do not create a
  separate local `Emits` type solely for the macro.
- Use `modelValue`/`defineModel` for two-way component state only when the parent truly owns the value.
- Use events for user intent (`submit`, `reset`, `delete`, `applyPreset`) rather than leaking DOM details.
- Keep prop names domain-specific and avoid boolean prop pairs that can conflict.
- Use `withDefaults` for stable display defaults instead of scattering fallback expressions in the template.

## Slots And Public Instance APIs

- Declare every named slot with `defineSlots`, including optional slots and slot
  props. Use `() => VNode[]` for slots without props.
- Assign the macro result to `slots` when the template needs to test slot
  presence, then use `slots.default`, `slots.footer`, or bracket access for
  hyphenated names.
- Do not use implicit `$props`, `$emit`, `$emits`, or `$slots` in templates.
  Declare the contract in `<script setup>` and use the declared prop, `emit`, or
  `slots` binding directly.
- Use `defineExpose` only for the smallest imperative API a parent genuinely
  needs, such as refreshing an independently owned panel.

## Reactivity

- Prefer `shallowRef` for primitives, opaque values, and data updated by replacing
  the root value. Use `ref` when nested object or array mutations must be reactive.
- Use `computed` for derived state. Do not mirror props into state unless local mutation is required.
- Use `watch` only for side effects or synchronization. Prefer computed values for derivation.
- Use lifecycle hooks for real lifecycle work, not for ordinary initialization that can happen synchronously.
- Prefer composables for browser APIs, async workflows, validation engines, media handling, storage, and reusable state.

## Template Style

- Keep templates declarative and scan-friendly.
- Reference props directly by name in templates, such as `foobar`, without a `props.` prefix like `props.foobar`.
- Keep `props.foo` access in scripts when the props object improves clarity; do not
  destructure props merely to satisfy the template rule.
- Prefer `v-if`/`v-else` states that show empty, loading, error, and success clearly.
- Use stable keys, usually domain IDs.
- Keep slot forwarding and dynamic slots typed and localized.
- Avoid heavy expressions in templates. Move non-trivial logic to computed values or small functions.
- Use project UI components and icon conventions before adding raw markup.
- Keep accessibility in the component contract: labels, button types, keyboard behavior, focus, and disabled states.

## Dialog And Drawer Components

- Prefer the application's existing command-style alert, confirm, or prompt API
  for simple confirmations. Create a component when the surface owns a form,
  list, media editor, fullscreen interaction, or another meaningful UI contract.
- Prefix independently reusable component symbols with their surface role, such
  as `DialogResourcePicker` and `DrawerResourcePreview`; avoid suffix forms such
  as `ResourcePickerDialog`.
- Inside a dedicated `dialogs/` or `drawers/` directory, omit the redundant role
  from the filename (`dialogs/ResourcePicker.vue`) while retaining it in the
  imported component symbol (`DialogResourcePicker`). Outside such a directory,
  keep the role prefix in both filename and symbol.
- A component that happens to render a dialog or drawer internally is not
  automatically a dialog/drawer component; name it for its primary public
  responsibility.
- After a component rename, search for the old filename, import symbol, exports,
  and template tags so no stale reference remains.

## Styling

- Prefer the repository's existing styling system: UnoCSS, Tailwind CSS, Nuxt UI `ui` props, scoped CSS, or local design tokens.
- When UnoCSS or Tailwind CSS is installed, prefer atomic utility classes over writing styles in a `<style>` block.
- Keep class lists organized by layout, spacing, size, color, and state according to the formatter/linter.
- Avoid component-local CSS when utilities or existing primitives express the design more directly.
- Add stable dimensions for fixed-format controls, grids, cards, canvas/SVG areas, previews, and toolbars.
- When UnoCSS is configured, use the `unocss` skill for utility syntax,
  extraction, units, shortcuts, and responsive variants. Use `theme-color` for
  semantic color tokens and light/dark-theme decisions.

## Anti-Patterns

- Mutating props, mirroring derived values with watchers, or hiding side effects
  inside computed values.
- Accessing `props.foo` in templates or moving non-trivial logic into template
  expressions.
- Adding a style block that duplicates installed utility classes or existing UI
  primitives.

## Delivery Check

For component changes, run the relevant checks:

- `pnpm lint`
- `pnpm typecheck` or `pnpm vue-tsc --noEmit`
- `pnpm test` for logic-heavy components/composables
- Browser inspection for visual or interactive changes.
- Confirm each `ref` or `shallowRef` matches how its value is mutated.
- Review template expressions for `props.` access and use direct prop names.
