---
name: typescript-npm-package
description: Create, modify, review, or prepare TypeScript npm packages in ntnyq's preferred style. Use for package scaffolding, exports, tsdown builds, declaration output, Vitest tests, package.json metadata, monorepos, or Chinese prompts such as "typescript npm 包开发", "TS 包", "npm package", or "库开发". Use release-engineering instead for live publishing, tags, releases, dist-tags, or promotion.
---

# TypeScript NPM Package

Use this skill for library/package work intended to be consumed by other projects. Optimize for a small public API, strict types, ESM-first packaging, and predictable release checks.

**Core constraint:** Treat the public entry points, declarations, metadata, and
packed artifact as the package contract; source files are implementation details.

## Scope

- Apply this skill to TypeScript package APIs, builds, declarations, metadata,
  tests, export maps, and package-oriented monorepos.
- Keep application feature work outside this skill. Use `release-engineering` for
  live publishing, tags, release creation, dist-tags, and promotion workflows.

## Workflow

- [ ] Inspect package and workspace metadata, TypeScript/build/test config,
      public entries, tests, and sibling packages.
- [ ] Define the consumer-facing runtime and type contract before changing the
      implementation or export map.
- [ ] Implement the smallest cohesive change while preserving the repository's
      module format, package manager, scripts, and naming style.
- [ ] Update tests, declarations, docs, metadata, and build entries that expose
      or verify the changed contract.
- [ ] Build and inspect the package, then run the delivery checks.

## Package Defaults

- Prefer pnpm, ESM, strict TypeScript, and `src/index.ts` as the main public entry when appropriate.
- Prefer `tsdown` for builds, with `clean: true`, declaration output, and explicit entries.
- Prefer `tsgo --noEmit` or the project's existing typecheck command.
- Prefer Vitest for unit tests and snapshots only when they improve the review surface.
- Prefer `oxlint`/`oxfmt` for small libraries when already present; use `@ntnyq/eslint-config` when the project uses ESLint.
- Include `files: ["dist"]`, `type: "module"`, `sideEffects: false` when true, `publishConfig.access`, and precise `exports`.
- For Node-targeted packages, set engines consistently with the repo, commonly `>=22.13.0` or `^22.13.0 || >=24`.

## API Design

- Keep the public API narrow and explicit. Re-export only stable types and functions.
- Design types as part of the API, not an afterthought. Export option/result types that users need.
- Avoid runtime dependencies unless they materially reduce complexity or match the package's domain.
- Use peer dependencies for host frameworks such as Vue, Vite, Rollup, Webpack, or unplugin integrations when consumers must provide them.
- Keep implementation modules internal unless there is a clear consumer use case.

## Implementation Pattern

For a simple package, prefer:

```text
src/
  index.ts
tests/
  index.test.ts
package.json
tsconfig.json
tsdown.config.ts
vitest.config.ts
```

For multi-entry packages or plugins, mirror the export map with explicit source entries and tests for each public entry.

## Anti-Patterns

- Exporting internal modules because they are convenient to import in tests.
- Adding a runtime dependency without checking whether a peer, dev dependency,
  or small local implementation better matches the consumer contract.
- Publishing, tagging, or changing release channels as incidental package work.

## Delivery Check

Run the repository's release-check equivalent when possible:

- `pnpm format:check`
- `pnpm lint`
- `pnpm typecheck`
- `pnpm test`
- `pnpm build`
- `pnpm pack --dry-run` when package metadata or exports changed.
- Confirm public runtime entries and exported types resolve from built output.
