---
name: typescript-style
description: Apply ntnyq's TypeScript coding standards while writing, refactoring, or reviewing TS/TSX/MTS/CTS code. Use for type design, strictness, module boundaries, tests, lint fixes, or Chinese prompts such as "TypeScript 编程规范", "TS 代码风格", "类型设计", or "重构 TS".
---

# TypeScript Style

Use this skill to make TypeScript code fit ntnyq's personal style: strict, small, explicit, ESM-first, and easy to verify.

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

## Formatting And Tooling

- Respect the repo's configured formatter and linter. Common local defaults are `oxfmt`, `oxlint`, `@ntnyq/eslint-config`, `perfectionist`, and strict tsconfig presets.
- Do not manually fight generated formatting or import ordering.
- Prefer `pnpm` scripts already present in `package.json`.

## Verification

After non-trivial changes, run the smallest relevant set:

- `pnpm lint`
- `pnpm typecheck`
- `pnpm test`
- `pnpm build` when emitted output or package boundaries changed.
