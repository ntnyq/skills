---
name: project-structure
description: Organize files and directories in ntnyq's preferred project structure for TypeScript packages, Vue/Vite apps, Nuxt apps, monorepos, business apps, docs, tests, scripts, and playgrounds. Use when creating, moving, or reviewing project layout, module boundaries, folder names, or Chinese prompts such as "项目目录结构", "目录组织", "文件放哪里", "项目结构", or "模块结构".
---

# Project Structure

Use this skill when deciding where files belong or shaping a new project. Prefer the current repository's structure first; introduce new folders only when they clarify ownership or match an established local pattern.

**Core constraint:** Put code near its narrowest real owner and introduce a new
structural convention only when it clarifies ownership or matches the repository.

## Scope

- Apply this skill to file placement, directory layout, workspace boundaries,
  module ownership, and deliberate file moves.
- Keep code-level style and product behavior outside this skill; use framework
  conventions to inform placement without redesigning the application by default.

## Workflow

- [ ] Inspect `package.json`, workspace and framework config, the top-level tree,
      and sibling folders.
- [ ] Classify the project and identify the narrowest owner for each affected file.
- [ ] Choose the smallest layout change that fits an existing convention or
      establishes a clearly justified boundary.
- [ ] Update imports, exports, routes, auto-import assumptions, tests, and config
      paths affected by the move.
- [ ] Run the delivery checks for every changed boundary.

## TypeScript Package Layout

Prefer this shape for single packages:

```text
src/
  index.ts
tests/
scripts/
docs/
playground/
```

- Use `src/index.ts` as the public entry for simple packages.
- Split `src/` by domain when the package grows: `array/`, `object/`, `string/`, `types/`, `core/`, `utils/`, `constants/`.
- Keep `tests/` at the root for package-level tests unless the repo colocates tests.
- Use `scripts/` for release/build/generation helpers.
- Use `playground/` for manual integration checks or demos.
- Use `docs/` only for actual documentation sites or substantial guides.

## Multi-Package Repositories

- Use `packages/` for workspace packages.
- Mirror package purpose in names and folders, such as `packages/components`, `packages/utils`, `packages/table`.
- Keep package-local `src`, `tests`, `tsdown.config.ts`, and `package.json` inside each package unless the repo centralizes config.
- Use root-level scripts for cross-package workflows and package-local scripts for package-specific checks.

## Vue And Vite App Layout

Prefer this shape for plain Vue/Vite apps:

```text
src/
  components/
  composables/
  constants/
  pages/
  stores/
  types/
  utils/
```

- Use `components/` for reusable UI pieces.
- Use `composables/` for `use*` stateful or browser/API logic.
- Use `pages/` for route-level views.
- Use `stores/` only for shared state that needs a store.
- Use `types/` for shared domain and API types.
- Use `utils/` for framework-light helpers.
- Add domain subfolders only after a feature has multiple files.

## Nuxt App Layout

Prefer Nuxt's `app/` structure when present:

```text
app/
  components/
  composables/
  constants/
  layouts/
  pages/
  styles/
  types/
  utils/
```

- Follow Nuxt auto-import conventions.
- Keep app-level UI in `app/components`, route views in `app/pages`, shared composables in `app/composables`.
- Put global CSS or Tailwind/UnoCSS entry files in `app/styles` or `assets/css` according to the repo.
- Do not add `src/` beside `app/` unless the project already uses that split.

## Business App Layout

For larger admin or business apps, prefer ownership-oriented folders:

```text
src/
  app/
  engines/
  modules/
  shared/
  vendors/
```

- Use `app/` for shell concerns: router, layout, pages, styles, stores, i18n.
- Use `modules/` for domain features and route groups.
- Use `shared/` for cross-module API clients, common components, constants, composables, types, and utilities.
- Use `engines/` for reusable internal systems such as form/table/detail renderers.
- Use `vendors/` for wrapped third-party integrations such as chart or graph systems.
- Keep module-local pages, i18n, components, types, and API code inside the owning module when possible.

## Browser Extension Layout

- Follow the framework convention first, commonly `entrypoints/`, `components/`, `composables/`, `constants/`, `stores/`, `types/`, and `utils/`.
- Keep extension entry code thin and move reusable logic to shared folders.

## Placement Rules

- Keep framework-agnostic utilities out of component folders.
- Keep generated output such as `dist`, `.nuxt`, `.output`, and `.wxt` out of source decisions.

## Anti-Patterns

- Creating speculative folders, layers, or shared modules before they have real
  contents or a second caller.
- Moving unrelated files to make a targeted structural change look symmetrical.
- Using barrels or catch-all directories to hide unclear ownership.

## Delivery Check

After moving files, run the relevant checks:

- Confirm every affected file has one clear owner and no imports still reference
  the old location.
- `pnpm lint`
- `pnpm typecheck`
- `pnpm test`
- `pnpm build` when entry points, auto imports, routes, or package exports changed.
