---
name: vue-sfc-style
description: Apply ntnyq's Vue single-file component coding standards. Use for writing, refactoring, or reviewing .vue files, props/emits/models/slots, composables, template structure, UI state, or Chinese prompts such as "vue-sfc 编程规范", "Vue 组件规范", "SFC 风格", or "重构 Vue 组件".
---

# Vue SFC Style

Use this skill for `.vue` component work. Prefer readable component contracts, typed Composition API, and templates that make user flows clear.

## Component Shape

- Prefer `<script setup lang="ts">`.
- Order blocks as script, template, then style when styles are needed.
- Keep component contracts near the top: imports, props, emits, model, slots, refs/state, computed values, functions, watchers/lifecycle, expose.
- Use `defineProps<T>()`, `withDefaults`, `defineEmits<T>()`, `defineModel<T>()`, and `defineExpose()` with explicit public types.
- Use `useTemplateRef()` for template refs when available in the project.
- Keep a component focused. Extract child components only when it reduces template noise, isolates a meaningful UI unit, or enables reuse.

## Props, Emits, Models

- Type props and emits with object/tuple syntax.
- Name the component props type `Props`; do not include the component name, such as `AppProps`.
- Use `modelValue`/`defineModel` for two-way component state only when the parent truly owns the value.
- Use events for user intent (`submit`, `reset`, `delete`, `applyPreset`) rather than leaking DOM details.
- Keep prop names domain-specific and avoid boolean prop pairs that can conflict.
- Use `withDefaults` for stable display defaults instead of scattering fallback expressions in the template.

## Reactivity

- Use `ref` for primitives and replaceable values.
- Use `computed` for derived state. Do not mirror props into state unless local mutation is required.
- Use `watch` only for side effects or synchronization. Prefer computed values for derivation.
- Use lifecycle hooks for real lifecycle work, not for ordinary initialization that can happen synchronously.
- Prefer composables for browser APIs, async workflows, validation engines, media handling, storage, and reusable state.

## Template Style

- Keep templates declarative and scan-friendly.
- Reference props directly by name in templates, such as `foobar`, without a `props.` prefix like `props.foobar`.
- Prefer `v-if`/`v-else` states that show empty, loading, error, and success clearly.
- Use stable keys, usually domain IDs.
- Keep slot forwarding and dynamic slots typed and localized.
- Avoid heavy expressions in templates. Move non-trivial logic to computed values or small functions.
- Use project UI components and icon conventions before adding raw markup.
- Keep accessibility in the component contract: labels, button types, keyboard behavior, focus, and disabled states.

## Styling

- Prefer the repository's existing styling system: UnoCSS, Tailwind CSS, Nuxt UI `ui` props, scoped CSS, or local design tokens.
- When UnoCSS or Tailwind CSS is installed, prefer atomic utility classes over writing styles in a `<style>` block.
- Keep class lists organized by layout, spacing, size, color, and state according to the formatter/linter.
- Avoid component-local CSS when utilities or existing primitives express the design more directly.
- Add stable dimensions for fixed-format controls, grids, cards, canvas/SVG areas, previews, and toolbars.

## Verification

For component changes, run the relevant checks:

- `pnpm lint`
- `pnpm typecheck` or `pnpm vue-tsc --noEmit`
- `pnpm test` for logic-heavy components/composables
- Browser inspection for visual or interactive changes.
