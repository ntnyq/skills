---
name: typescript-style
description: Apply ntnyq's TypeScript coding standards while writing, refactoring, or reviewing TS/TSX/MTS/CTS code. Use for type design, strictness, module boundaries, tests, lint fixes, or Chinese prompts such as "TypeScript 编程规范", "TS 代码风格", "类型设计", or "重构 TS".
---

# TypeScript Style

Use this skill to make TypeScript code fit ntnyq's personal style: strict, small, explicit, ESM-first, and easy to verify.

**Core constraint:** Preserve runtime behavior at typed and external boundaries;
do not silence uncertainty with assertions or unrelated refactors.

## Scope

- Apply this skill to TypeScript implementation, type design, module cohesion,
  error handling, and code-level review.
- Keep directory layout, package publication, and Vue template conventions in
  their dedicated skills unless they directly affect the TypeScript change.

## Workflow

- [ ] Inspect nearby code, configured tooling, public APIs, and external input
      boundaries before editing.
- [ ] State the behavior and invariants that must remain true, including failure
      behavior and type-level contracts.
- [ ] Implement the smallest cohesive change with explicit boundaries and
      locally inferred types.
- [ ] Review errors, side effects, exports, and assertions, then run the delivery
      checks appropriate to the changed surface.

## Core Rules

- Follow existing local conventions before applying generic preferences.
- Prefer strict TypeScript and let inference work locally, but annotate public APIs, exported constants, callbacks, and complex generics.
- Use `type` imports and exports for type-only symbols.
- Prefer named exports for libraries and shared modules.
- Keep modules small and cohesive. Split only when it improves reading, testing, or API boundaries.
- Prefer early returns over nested conditionals.
- Prefer plain functions for reusable logic. Add classes only when identity, lifecycle, inheritance, or encapsulated state is genuinely useful.
- Avoid `any`. If unavoidable, isolate it near the boundary and explain it with a short comment.
- Prefer `unknown` plus narrowing for external input.
- Avoid non-null assertions unless the invariant is obvious from the same scope.

## Naming

- Use descriptive names that encode domain intent, not implementation trivia.
- Use `is*`, `has*`, `can*`, `should*` for booleans.
- Use `to*`, `parse*`, `create*`, `resolve*`, `normalize*`, `format*`, `get*`, and `set*` consistently with behavior.
- Avoid vague utility names such as `handleData`, `process`, `helper`, or `utils` when a domain verb is available.

## Types

- Prefer discriminated unions for state machines, parse results, command variants, and async state.
- Prefer `interface` for object shapes designed for extension; prefer `type` for unions, mapped types, aliases, and function signatures.
- Keep generic parameters meaningful and constrained.
- Use `satisfies` to validate object literals without widening away useful inference.
- Avoid exporting deep implementation types unless users need them.

## Error And Boundary Handling

- Validate external data at boundaries: CLI args, env vars, JSON, network responses, file contents, browser APIs, and plugin hooks.
- Preserve original errors when adding context.
- Do not silently swallow failures unless the feature is explicitly best-effort.
- Keep side effects near the boundary and pure logic in testable functions.

## Comments And Documentation

- Add comments for contracts, domain meaning, invariants, and non-obvious flow;
  do not narrate syntax that the code already makes clear.
- When a declaration needs JSDoc or TSDoc, use the multiline form even for a
  single sentence:

  ```ts
  /**
   * Resolves the effective configuration for one workspace
   */
  ```

- Follow the repository's language and punctuation style. Do not rewrite
  unrelated comments merely to make their formatting uniform.
- Document exported APIs whose contract is not obvious and internal helpers
  that represent a meaningful workflow step or branch.
- For an exported enum-like `as const` object, document the purpose of the
  collection and the domain meaning of each member declared directly in that
  object. Do not repeat comments for members inherited through object spread.
- When adding documentation to shared type fields, verify the meaning from API
  contracts, UI labels, validation, tests, constants, and call sites. Leave an
  uncertain field undocumented instead of guessing or adding filler such as
  "value", "type", or "status".
- Reference a finite-value constant from a field comment only after confirming
  that the constant exists and governs that exact field in the same domain.

## Formatting And Tooling

- Respect the repo's configured formatter and linter. Common local defaults are `oxfmt`, `oxlint`, `@ntnyq/eslint-config`, `perfectionist`, and strict tsconfig presets.
- Do not manually fight generated formatting or import ordering.
- Prefer `pnpm` scripts already present in `package.json`.

## Anti-Patterns

- Using `any`, casts, non-null assertions, or ignored diagnostics merely to make
  the checker pass.
- Mixing a focused behavior change with broad renaming, module movement, or
  formatting cleanup.
- Catching an error without preserving context, surfacing failure, or explicitly
  defining best-effort behavior.

## Delivery Check

After non-trivial changes, run the smallest relevant set:

- `pnpm lint`
- `pnpm typecheck`
- `pnpm test`
- `pnpm build` when emitted output or package boundaries changed.
- Confirm public types, boundary validation, and failure behavior match the
  intended runtime semantics.
