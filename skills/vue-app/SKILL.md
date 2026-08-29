---
name: vue-app
description: Build, modify, review, or debug application-level Vue 3 features in ntnyq's preferred style. Use for Vue/Nuxt/Vite app flows, routing, state, framework config, or Chinese prompts such as "Vue app 开发", "Vue 应用", "Nuxt 应用", or "前端页面". Use vue-sfc-style instead for isolated .vue component conventions.
---

# Vue App

Use this skill when working on an application rather than a published library. Prefer the repository's existing stack and conventions over introducing a new framework or design system.

**Core constraint:** Preserve the application's existing stack and make the
requested user flow complete before introducing new abstractions or tools.

## Scope

- Apply this skill to application-level feature composition, routing, state,
  UI flows, framework config, and end-to-end behavior in Vue-based apps.
- Use `vue-sfc-style` for detailed `.vue` contracts, reactivity, templates, and
  styling. Keep published-library concerns outside this skill.

## Focused References

- Read [page architecture](references/page-architecture.md) when building or
  reviewing route pages, CRUD flows, list/form/detail composables, tabs, or
  multi-step forms.
- Read [browser runtime boundaries](references/browser-runtime.md) when using
  secure-context browser APIs, generating client identifiers, or coordinating
  runtime logging with user-facing error notifications.

## Workflow

- [ ] Inspect package, workspace, framework, lint, and TypeScript config plus the
      affected routes, components, composables, stores, and tests.
- [ ] Identify the runtime and map each non-trivial component's responsibility,
      props/emits contract, state owner, and side-effect boundary.
- [ ] Implement the complete requested flow with the existing UI, routing, state,
      styling, and testing systems.
- [ ] Review loading, empty, error, disabled, success, and confirmation states,
      then run the delivery checks and inspect the actual page.

## Defaults

- Use Vue 3 with Composition API, `<script setup lang="ts">`, and strict TypeScript.
- Prefer Nuxt conventions in Nuxt apps: auto imports, `app/` structure, `use*` composables, modules, `nuxt typecheck`, and `nuxt prepare`.
- Prefer Vite conventions in plain Vue apps: small modules, explicit imports, focused `vite.config.ts`, and Vitest for tests.
- Use VueUse when it removes lifecycle, browser API, storage, media, event, or async-state boilerplate.
- Prefer existing UI systems first. Common local choices include Nuxt UI, Element Plus, UnoCSS, Tailwind CSS, Iconify/Lucide icons, and project-local components.
- Keep shared logic in composables or utilities only when it is reused or makes the component easier to read.
- Keep state local until cross-route, cross-component, persistence, or derived-state complexity justifies a store.
- Keep route pages as composition surfaces. Move reusable reactive workflows and
  effects to composables and keep pure payload/default-value transformations in
  utilities.

## Implementation Style

- Make the requested user flow usable on the first screen when building tools or apps.
- Build complete UI states: empty, loading, error, disabled, active, success, and destructive confirmations when relevant.
- Use stable dimensions for boards, toolbars, grids, cards, media previews, and controls so text or dynamic data cannot shift the layout unexpectedly.
- Use icons for common commands and concise text labels for domain actions.
- Avoid landing-page filler unless the user explicitly asks for a marketing page.
- Do not introduce new formatting, linting, router, state, CSS, or testing tools unless the project already uses them or the user asks.

## Anti-Patterns

- Putting orchestration, multiple independent UI sections, and reusable behavior
  into one route or root component.
- Introducing a store, router, design system, CSS framework, or testing tool before
  the requested flow requires it.
- Delivering only the happy path or replacing product UI with landing-page filler.

## Delivery Check

Run the smallest meaningful validation set from the repository:

- `pnpm lint`
- `pnpm typecheck`
- `pnpm test`
- `pnpm build`
- For Nuxt: include `pnpm nuxt prepare` or the project's `postinstall`/typecheck script when needed.
- For visual work: run the dev server and inspect the actual page in a browser at desktop and mobile sizes.
- Confirm component contracts and state ownership remain explicit across the
  complete user flow.
