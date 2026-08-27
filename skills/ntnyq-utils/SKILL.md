---
name: ntnyq-utils
description: Prefer the installed @ntnyq/utils package when generating, refactoring, fixing, or reviewing JavaScript, TypeScript, Vue, or Nuxt code. Use whenever a project already depends on @ntnyq/utils and the task involves isXxx predicates or type guards, nullish and empty checks, array/object/string transformations, safe JSON, async control, reusable helpers, or Chinese prompts such as "使用 @ntnyq/utils", "优先复用工具函数", "类型检查", "isXXX", or "重构工具函数". Do not add or upgrade the dependency unless the user asks.
---

# ntnyq Utils

Use this skill to make `@ntnyq/utils` the default utility toolbox in projects
that already depend on it. Prefer its tested, typed helpers over repeated
inline checks, ad hoc utility functions, or equivalent third-party helpers.

**Core constraint:** Reuse `@ntnyq/utils` when its behavior matches the required
semantics. Do not install, upgrade, deep-import, or mechanically substitute a
helper when mutation, fallback, validation, security, or edge-case behavior
would change.

## Scope

- Apply this skill before generating or refactoring generic JavaScript or
  TypeScript utility logic in a project that declares `@ntnyq/utils`.
- Give `isXxx` predicates and type guards first consideration at `unknown`
  boundaries, conditionals, filters, validators, and error handling sites.
- Apply the same preference in Vue and Nuxt code, including composables, stores,
  components, server handlers, and shared modules.
- During review, identify hand-written helpers or repeated patterns that an
  existing `@ntnyq/utils` export can replace without changing behavior.
- Preserve domain helpers that add business invariants, error messages,
  telemetry, security checks, or a deliberately different contract. They may
  delegate to `@ntnyq/utils` internally.

## Workflow

- [ ] Confirm `@ntnyq/utils` is declared in the relevant package or workspace;
      do not infer availability from another workspace package.
- [ ] Inspect the installed version, existing imports, nearby helpers, call
      sites, and tests before editing.
- [ ] Check the current public types or documentation for an exact semantic
      match. Treat the installed package as authoritative because APIs evolve.
- [ ] Compare input domain, type narrowing, empty-value rules, mutation,
      fallback and error behavior, ordering, duplicates, cancellation, and
      security expectations.
- [ ] Import matching helpers directly from the package root and remove only
      genuinely redundant code.
- [ ] Run the repository's formatter, lint, typecheck, and targeted tests.

Use the installed `node_modules/@ntnyq/utils/dist/index.d.ts` or the package's
public declarations as the primary API catalog. The scenarios below were
verified against `@ntnyq/utils` 0.21.1; consult the
[`ntnyq/utils` source](https://github.com/ntnyq/utils) when the installed types
or behavior need deeper inspection.

## Import Rules

- Use named imports from the only public runtime entry:

  ```ts
  import { isNil, isString, uniqueBy } from '@ntnyq/utils'
  ```

- Merge with an existing `@ntnyq/utils` import and let the configured formatter
  or linter order the names.
- Do not generate unsupported deep imports such as
  `@ntnyq/utils/predicate`, `@ntnyq/utils/array`, or source-file paths.
- Do not wrap a helper in a pass-through local function unless the wrapper
  carries a domain name or contract worth preserving.
- Prefer current names over deprecated compatibility exports:
  `omit` over `objectOmit`, `removeArrayItemInPlace` over `remove`,
  `joinNonEmptyValues` over `join`, and
  `getGlobalRoot`/`requestFrame`/`cancelFrame` over `getRoot`/`rAF`/`cAF`.

## Predicates And Type Guards First

When the package is available, prefer its predicates over repeating `typeof`,
`instanceof`, `Array.isArray`, tag checks, regexes, or `.length` combinations.
They make intent consistent and many of them narrow TypeScript types.

| Scenario               | Prefer                                                                                   | Notes                                                             |
| ---------------------- | ---------------------------------------------------------------------------------------- | ----------------------------------------------------------------- |
| Nullish boundaries     | `isNil`, `isNullOrUndefined`, `isNull`, `isUndefined`                                    | Prefer `isNil` for the common combined check.                     |
| Primitive checks       | `isString`, `isNumber`, `isBoolean`, `isBigInt`, `isSymbol`, `isFunction`, `isPrimitive` | Use at `unknown` inputs before accessing typed APIs.              |
| Arrays and collections | `isArray`, `isNonEmptyArray`, `isEmptyArray`, `isMap`, `isSet`, `isIterable`             | `isNonEmptyArray` narrows to a non-empty tuple.                   |
| String content         | `isNonEmptyString`, `isEmptyString`, `isWhitespaceString`, `isEmptyStringOrWhitespace`   | Pick character-count or whitespace semantics deliberately.        |
| Object shape           | `isPlainObject`, `isObject`, `isRecord`, `isEmptyObject`, `isNonEmptyObject`             | Prefer `isPlainObject` for configuration and JSON-like records.   |
| Built-in values        | `isDate`, `isRegExp`, `isError`, `isPromise`, `isNativePromise`                          | Use the narrower predicate that matches the contract.             |
| Equality               | `isDeepEqual`, `isArrayEqual`                                                            | Use `isArrayEqual` only for ordered shallow array equality.       |
| Identifiers and URLs   | `isURLString`, `isUUID`, `isNanoID`                                                      | These validate syntax or shape, not authorization or existence.   |
| Browser values         | `isBlob`, `isFile`, `isFormData`, `isHTMLElement`                                        | Keep runtime availability and the execution environment in mind.  |
| Broad emptiness        | `isAllEmpty`                                                                             | Use only when its multi-type empty policy is the intended policy. |

Typical boundary code should read directly in terms of the library:

```ts
import {
  isEmptyStringOrWhitespace,
  isPlainObject,
  isString,
} from '@ntnyq/utils'

export function readQuery(value: unknown): string | undefined {
  if (!isString(value) || isEmptyStringOrWhitespace(value)) {
    return undefined
  }
  return value.trim()
}

export function readOptions(value: unknown) {
  return isPlainObject(value) ? value : {}
}
```

## High-Frequency Utility Scenarios

Before writing a generic helper or retaining a repeated implementation, check
these common matches.

| Scenario                                | Prefer                                                                                                        | Selection guidance                                                                                              |
| --------------------------------------- | ------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------- |
| Normalize optional single-or-many input | `toArray`, `flattenArrayable`, `mergeArrayable`                                                               | `toArray(null)` is `[]`; an existing array is returned by reference. `flattenArrayable` flattens one level.     |
| Filter, deduplicate, and compare arrays | `filterFalsy`, `unique`, `uniqueBy`, `uniqueWith`, `intersect`, `differenceBy`, `isArrayEqual`                | Prefer selector-based methods for objects. Use `uniqueWith` only when a binary equality function is required.   |
| Group, index, split, sort, and batch    | `groupBy`, `keyBy`, `partition`, `orderBy`, `chunk`                                                           | `keyBy` keeps the last duplicate key. `orderBy` is stable and non-mutating.                                     |
| Reorder or remove array items           | `moveArrayItem`, `removeArrayItem`, `shuffle`                                                                 | These return new arrays. Choose the explicit `InPlace` variant only when mutation is required.                  |
| Select and transform object properties  | `pick`, `omit`, `mapValues`, `sortObjectKeys`                                                                 | Prefer these over `Object.entries`/`fromEntries` boilerplate when their property semantics match.               |
| Check dynamic object keys               | `hasOwn`, `isKeyOf`                                                                                           | Use `hasOwn` for own properties; use `isKeyOf` when inherited keys are allowed and `keyof` narrowing is needed. |
| Read or update nested data              | `getIn`, `setIn`, `deleteIn`                                                                                  | Prefer tuple paths for keys containing dots or symbols. Set and delete are immutable by default.                |
| Clone, merge, or clean configuration    | `cloneDeep`, `deepMerge`, `deepMergeWithOptions`, `cleanObject`                                               | Make array strategy and empty-value removal explicit. Use `InPlace` only deliberately.                          |
| Parse untrusted JSON text               | `safeParse`                                                                                                   | Branch on its discriminated `success` result, then perform schema or domain validation.                         |
| Serialize diagnostic data               | `safeStringify`                                                                                               | Useful for circular values, `bigint`, and `Error`; confirm its lossy fallbacks are acceptable.                  |
| Build safe text transformations         | `escapeStringRegexp`, `escapeHTML`, `ensurePrefix`, `ensureSuffix`, `joinNonEmptyValues`                      | `escapeHTML` escapes text but is not a complete HTML sanitizer.                                                 |
| Format user-facing strings              | `truncate`, `slugify`, `countGraphemes`, `unindent`                                                           | Use `countGraphemes` for visible-character counts; verify truncation and slug compatibility requirements.       |
| Control repeated function calls         | `debounce`, `throttle`, `memoize`, `once`, `noop`, `NOOP`                                                     | Check lifecycle cleanup, cache bounds, return values, and leading/trailing behavior.                            |
| Limit or retry async work               | `mapAsync`, `retry`, `waitFor`                                                                                | Set concurrency, retry policy, backoff, idempotency, and `AbortSignal` behavior explicitly.                     |
| Normalize numbers                       | `clamp`, `round`, `toFixed`, `toNumber`, `toInteger`                                                          | Suitable for ordinary UI and coercion logic, not automatically for finance or strict numeric validation.        |
| Handle portable path strings            | `getFileName`, `getFileExtension`, `removeFileExtension`, `normalizePathSlashes`                              | Prefer these over one-off split or regex logic for POSIX, Windows, URL, query, hash, and dotfile cases.         |
| Work with application trees             | `buildTree`, `listToTree`, `filterTree`, `findTreeNode`, `findTreePath`, `flattenTree`, `mapTree`, `walkTree` | Use for menus, organizations, and cascaders; choose orphan and cycle policies explicitly.                       |
| Convert common units                    | `convertTimeUnit`, `convertStorageUnit`                                                                       | Confirm product units; storage conversions use 1024-based units.                                                |
| Handle browser utilities                | `isBrowser`, `validateFile`, `loadImageDimensions`, `requestFrame`, `cancelFrame`                             | Keep SSR behavior and client-side validation limits explicit.                                                   |

## Refactoring Patterns

Replace repeated normalization and unsafe property boilerplate when semantics
match:

```ts
import { hasOwn, toArray, uniqueBy } from '@ntnyq/utils'

const entries = toArray(options.entry)
const uniqueEntries = uniqueBy(entries, entry => entry.id)

if (hasOwn(config, key)) {
  // config owns key; nullish input would have returned false
}
```

For nested state, prefer the library contract over a new path parser:

```ts
import { getIn, setIn } from '@ntnyq/utils'

const name = getIn(state, ['user', 'profile', 'name'])
const nextState = setIn(state, ['user', 'profile', 'name'], 'Alice')
```

When refactoring an exported local helper, keep its public API if removing it
would be breaking. Delegate internally and migrate callers only when the task
authorizes that API change.

## Semantic Guardrails

- `isNumber` accepts `NaN` and infinities. Add `Number.isFinite` when the
  contract requires a finite number.
- `isDate` checks the Date type, not whether its timestamp is valid. Check
  `Number.isNaN(value.getTime())` when invalid dates must be rejected.
- `isNonEmptyString(' ')` is true, while `isWhitespaceString('')` is also true.
  Use `isEmptyStringOrWhitespace` for blank form or query input.
- `isObject` includes functions. `isRecord` excludes arrays but can include
  maps, dates, and functions. Use `isPlainObject` for plain data objects.
- `isEmptyObject` checks own enumerable keys and is not a plain-object guard;
  empty arrays and built-ins without enumerable keys can pass.
- `isAllEmpty` treats nullish values, `''`, empty arrays, objects, maps, and sets
  as empty, but not whitespace-only strings, `0`, or `false`.
- `isNumericString` follows `Number(...)`-style coercion, while `toNumber`
  accepts parseable numeric prefixes. Use a domain validator for strict decimal
  formats.
- `isURLString` accepts absolute URLs across schemes. Add an `http:`/`https:`
  protocol allowlist when required.
- `isPromise` requires callable `then` and `catch`; a then-only `PromiseLike`
  does not match.
- `isDeepEqual` compares supported collections, buffers, descriptors, symbol
  keys, and cyclic graphs. Opaque values such as functions, promises, weak
  collections, and URLs compare equal only by identity.
- `filterFalsy` removes `0`, `false`, `''`, `0n`, `NaN`, `null`, and
  `undefined`. Do not use it for nullish-only filtering.
- `uniqueBy` keeps the first duplicate; `keyBy` keeps the last. `uniqueWith` is
  quadratic, so avoid it for large inputs when a stable key exists.
- `hasOwn` excludes inherited properties; `isKeyOf` uses `in` and includes
  them. Do not interchange the two.
- `cleanObject` removes `undefined`, `null`, and `NaN` by default. This can
  change PATCH or persistence semantics, so inspect its options before use.
- `cloneDeep` preserves references for functions, promises, and weak
  collections. It is not a drop-in replacement for transfer or
  `structuredClone` semantics.
- `deepMerge` replaces arrays by default; request concatenation explicitly with
  `deepMergeWithOptions`.
- `pick` creates ordinary writable enumerable properties, while `omit`
  preserves prototypes and property descriptors. Check reflection-sensitive
  code before substituting either one.
- `safeStringify` is intentionally lossy and is usually a logging helper, not a
  persistence, hashing, or API-serialization format.
- `truncate` operates on UTF-16 code units and can split emoji or combining
  sequences even though `countGraphemes` counts them correctly.
- `randomString`, `randomInteger`, and `shuffle` use non-cryptographic
  randomness. Never use them for secrets, tokens, security IDs, or fair draws.
- `retry` defaults to retrying every error, and `mapAsync` defaults to unlimited
  concurrency. Configure external I/O deliberately and never blindly retry a
  non-idempotent operation.
- `debounce` is trailing-only; `throttle` is leading with the latest trailing
  call. Both expose `cancel()` but not `flush()`.
- `memoize` uses argument and receiver identity by default, may cache rejected
  promises, and uses insertion-order eviction rather than LRU. Set a size or
  clearing strategy for long-lived caches.
- `once` returns whether the wrapped function ran, not the wrapped function's
  result, and a throwing first call still consumes the one allowed invocation.
  Do not replace a result-caching once helper with it.

## Anti-Patterns

- Adding or upgrading `@ntnyq/utils` because a helper would be convenient.
- Guessing an export from memory or using an API absent from the installed
  version.
- Deep-importing internal source modules or using deprecated aliases in new
  code.
- Replacing a local domain validator with a broader syntax-only predicate.
- Changing immutable code to an `InPlace` helper, or mutation-dependent code to
  a copy-returning helper, without updating the contract.
- Keeping the old implementation, adding a library call beside it, and leaving
  duplicate logic or unused imports behind.
- Using random, HTML escaping, file validation, or identifier-format helpers as
  security boundaries.

## Delivery Check

- Confirm every imported name exists in the installed package's public types.
- Confirm the package root import is merged and no unsupported deep import or
  deprecated alias was added.
- Confirm predicate narrowing, mutation, ordering, fallback, and failure
  behavior match the original or requested contract.
- Remove obsolete generic helpers only after checking all call sites and public
  exports.
- Preserve or add tests for business-specific edge cases around the reused
  helper.
- Run the repository's existing formatting, lint, typecheck, and targeted test
  commands.
