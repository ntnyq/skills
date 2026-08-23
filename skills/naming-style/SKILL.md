---
name: naming-style
description: Apply ntnyq's naming conventions for variables, constants, functions, composables, classes, interfaces, type aliases, generics, files, and exported APIs. Use when naming or reviewing identifiers, public API names, constants, type names, or Chinese prompts such as "变量命名", "常量命名", "类型命名", "命名规范", or "取名".
---

# Naming Style

Use this skill when choosing, reviewing, or refactoring names. Prefer names that reveal domain intent and match nearby code over rigid global rules.

**Core constraint:** Preserve domain meaning, nearby conventions, and public API
stability; a naming task does not authorize an unrelated rename or breaking
change.

## Scope

- Apply this skill to identifiers, file names, exported symbols, and deliberate
  rename work.
- Keep module placement, API behavior, and implementation architecture outside
  this skill unless they directly determine the name.

## Workflow

- [ ] Inspect nearby files and vocabulary before choosing or changing a name;
      local convention wins.
- [ ] Classify the symbol as public API, internal implementation, test-only,
      UI-only, or generated.
- [ ] Choose a domain-specific name with the appropriate verb, boolean prefix,
      type suffix, casing, and file/export relationship.
- [ ] For renames, update every in-scope reference and inspect the public API
      impact before applying the change.
- [ ] Run the delivery checks relevant to the affected surface.

## Variables And Functions

- Use `camelCase` for variables, refs, computed values, functions, methods, and local helpers.
- Prefer verb phrases for functions: `normalizeAttrName`, `matchesPattern`, `createAxiosClient`, `resolveOptions`, `getPendingKey`.
- Prefer noun phrases for values: `stream`, `error`, `componentProps`, `pendingMap`, `constantRoutes`.
- Use boolean prefixes: `is*`, `has*`, `can*`, `should*`, `supports*`, `enabled`.
- Use action prefixes consistently:
  - `get*` for cheap retrieval or derived lookup.
  - `set*` for assignment/mutation.
  - `create*` for new instances.
  - `resolve*` for turning user input/options into final config.
  - `normalize*` for shape cleanup.
  - `parse*` for syntax/string/data parsing.
  - `format*` for display strings.
  - `to*` for conversion.
  - `use*` for Vue composables.
- Avoid vague names such as `data`, `item`, `value`, `result`, `handler`, `helper`, `process`, and `utils` unless the scope is tiny and obvious.

## Constants

- Use `camelCase` for ordinary immutable values that behave like local variables.
- Use `SCREAMING_SNAKE_CASE` for true constants: protocol strings, regex tables, prefix lengths, default option objects, maps, enum-like values, and values that must stand out.
- Keep paired constants visually aligned by name, such as `V_BIND_PREFIX` and `V_BIND_PREFIX_LEN`.
- Use `DEFAULT_*` for default config objects or fallback values.
- Use `RE_*` or `r*` consistently with nearby regex naming. Prefer existing local style.

## Types

- Use `PascalCase` for interfaces, type aliases, classes, enums, and exported structural types.
- Use descriptive suffixes:
  - `Options` for user-provided configuration.
  - `ResolvedOptions` for normalized config.
  - `Result` for operation output.
  - `Context` for callback/plugin/render context.
  - `State` for mutable state.
  - `Payload` for request or event payloads.
  - `Config` for framework/client setup.
- Prefer `UseXxxOptions` for composable options, matching `useXxx`.
- Prefer domain names over generic names: `TableFetchParams`, `DetailRenderContext`, `RemoveAttrContext`.
- Keep single-letter generics only for obvious local type transforms. Use meaningful names such as `Left`, `Right`, `Objects`, `Union`, or `Base` for exported utility types.
- Avoid Hungarian-style prefixes except where the local project already uses them for domain models.

## Files And Exports

- Match file names to the main export: `normalizeAttrName` in a small domain module, `useMediaStream.ts` for a composable, `types.ts` for grouped local types.
- Use directory `index.ts` files for deliberate public aggregation, not as a dumping ground.
- Prefer named exports. Default exports should follow framework or tooling conventions.
- Keep public names stable, clear, and searchable.

## Anti-Patterns

- Imposing a new naming system when nearby code already uses a coherent one.
- Renaming exported, generated, or protocol-defined symbols as incidental cleanup.
- Performing a broad vocabulary rewrite during a narrowly scoped change.

## Delivery Check

- Rename all references with tooling when possible.
- Check tests, docs, snapshots, and export maps after public renames.
- Run `pnpm lint`, `pnpm typecheck`, and targeted tests after non-trivial renames.
