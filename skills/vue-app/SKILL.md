---
name: vue-app
description: Build, modify, review, or debug Vue 3 applications in ntnyq's preferred style. Use for Vue/Nuxt/Vite app work, UI features, composables, routing, state, build config, tests, or Chinese prompts such as "Vue app 开发", "Vue 应用", "Nuxt 应用", or "前端页面".
---

# Vue App

Use this skill when working on an application rather than a published library. Prefer the repository's existing stack and conventions over introducing a new framework or design system.

## First Pass

1. Inspect `package.json`, `pnpm-workspace.yaml`, `nuxt.config.*`, `vite.config.*`, `eslint.config.*`, `tsconfig*.json`, and nearby components before editing.
2. Identify whether the app is Nuxt, Vite Vue, WXT extension, or another Vue-based runtime.
3. Follow existing directories such as `app/`, `src/`, `components/`, `composables/`, `stores/`, `pages/`, `layouts/`, `utils/`, and `types/`.
4. Prefer `pnpm` commands and the scripts already defined by the project.

## Defaults

- Use Vue 3 with Composition API, `<script setup lang="ts">`, and strict TypeScript.
- Prefer Nuxt conventions in Nuxt apps: auto imports, `app/` structure, `use*` composables, modules, `nuxt typecheck`, and `nuxt prepare`.
- Prefer Vite conventions in plain Vue apps: small modules, explicit imports, focused `vite.config.ts`, and Vitest for tests.
- Use VueUse when it removes lifecycle, browser API, storage, media, event, or async-state boilerplate.
- Prefer existing UI systems first. Common local choices include Nuxt UI, Element Plus, UnoCSS, Tailwind CSS, Iconify/Lucide icons, and project-local components.
- Keep shared logic in composables or utilities only when it is reused or makes the component easier to read.
- Keep state local until cross-route, cross-component, persistence, or derived-state complexity justifies a store.

## Implementation Style

- Make the requested user flow usable on the first screen when building tools or apps.
- Build complete UI states: empty, loading, error, disabled, active, success, and destructive confirmations when relevant.
- Use stable dimensions for boards, toolbars, grids, cards, media previews, and controls so text or dynamic data cannot shift the layout unexpectedly.
- Use icons for common commands and concise text labels for domain actions.
- Avoid landing-page filler unless the user explicitly asks for a marketing page.
- Do not introduce new formatting, linting, router, state, CSS, or testing tools unless the project already uses them or the user asks.

## Verification

Run the smallest meaningful validation set from the repository:

- `pnpm lint`
- `pnpm typecheck`
- `pnpm test`
- `pnpm build`
- For Nuxt: include `pnpm nuxt prepare` or the project's `postinstall`/typecheck script when needed.
- For visual work: run the dev server and inspect the actual page in a browser at desktop and mobile sizes.
