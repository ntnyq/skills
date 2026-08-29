---
name: unocss
description: Configure, write, migrate, or review UnoCSS utilities in Vue, Nuxt, Vite, JSX, and template code using ntnyq's conventions. Use whenever UnoCSS, uno.config, shortcuts, presets, variant groups, responsive utilities, arbitrary values, utility extraction, or migration from scoped CSS/media queries is involved.
---

# UnoCSS

Use this skill for intentional utility-first styling with UnoCSS. UnoCSS is
preset-driven, so inspect the project's actual configuration before assuming
Wind syntax, Attributify, icons, typography, directives, or variant groups.

**Core constraint:** Generate only syntax the configured presets, transformers,
extractors, shortcuts, and content pipeline can recognize.

## Workflow

- [ ] Inspect `uno.config.*` or `unocss.config.*`, integration plugins/modules,
      presets, transformers, theme, shortcuts, rules, safelist, and content globs.
- [ ] Inspect nearby templates and the project's formatter/linter class ordering.
- [ ] Reuse an existing component or semantically exact shortcut before adding
      utilities, rules, or CSS.
- [ ] Verify static extraction and inspect responsive/theme states in a production
      build when utilities are dynamic or configuration changes.

## Selection Order

1. Reuse an existing component or shortcut when its complete contract matches.
2. Prefer standard utilities from the configured preset.
3. Use the preset's spacing and sizing scale when it represents the design value
   exactly.
4. Use an explicit unit or arbitrary value when the scale cannot express the
   value without changing the design.
5. Use scoped CSS for complex selectors, keyframes, third-party overrides, or
   cross-node state that utilities cannot express clearly.

Do not enable Attributify or another preset/transformer merely because an example
uses its syntax. Add configuration only when the user wants that capability and
the repository accepts the resulting syntax and build impact.

## Typography, Units, And Spacing

- With Wind-compatible presets, prefer semantic font weights such as
  `font-medium`, `font-semibold`, and `font-bold` over numeric aliases.
- In this house style, use `lh-*` for line height instead of mixing it with
  `leading-*` forms.
- Prefer scale utilities when values map exactly: `gap-1` for 4px, `gap-2` for
  8px, `p-3` for 12px, `gap-4` for 16px, `gap-6` for 24px, and `gap-8` for 32px
  under the standard Wind scale.
- Use semantic dimensions such as `w-full`, `w-1/2`, `h-screen`, `min-h-screen`,
  and configured typography sizes when they express the intent.
- Collapse identical edges with utilities such as `inset-0` instead of listing
  four separate edge declarations.
- Avoid fractional pixel values in utility classes, inline styles, and new CSS.
  Snap ordinary layout to the project's spacing grid; reserve deliberate
  precision for SVG, Canvas, print, coordinate, or third-party contracts.
- When an exact integer value is necessary, use direct unit syntax supported by
  the preset (for example `w-120px`) before a bracketed arbitrary value.

## CSS Variables And Arbitrary Values

- Use the configured variable shorthand such as `text-$text-primary` or
  `bg-$bg-secondary` when available.
- Reserve brackets for functions, complex expressions, spaces, or required type
  hints, such as `min-h-[calc(100vh-120px)]` and
  `grid-cols-[1fr_10px_max-content]`.
- Follow UnoCSS underscore escaping for spaces inside arbitrary values.
- Use the `theme-color` skill to decide whether a value belongs to a semantic
  theme token, a fixed palette, or a rendering-boundary palette.

## Variants And Responsive Design

- Use variant groups only when the transformer is configured and one prefix
  applies to at least two utilities clearly, for example
  `hover:(bg-primary text-white)`. Keep a single variant as `hover:*`.
- Prefer targeted transitions such as `transition-colors` or
  `transition-transform` over `transition-all` when the changing properties are
  known.
- Use important variants only for a confirmed cascade conflict, commonly a
  third-party override; do not make `!` the default.
- Build responsive layouts mobile-first: unprefixed utilities describe the
  narrow-screen base, then configured `sm:`, `md:`, `lg:`, `xl:`, or `2xl:`
  variants enhance wider screens.
- Prefer configured built-in breakpoints over arbitrary thresholds. Keep a custom
  breakpoint only when it is a real product/third-party contract or testing
  proves the nearest built-in breakpoint fails.
- When migrating a desktop-first `max-width` query, move narrow styles to the
  unprefixed base and add the wider layout at the nearest correct breakpoint.

## Static Extraction

UnoCSS must see complete candidate class names at build time:

```ts
const toneClassMap = {
  danger: 'border-$danger-color text-$danger-color',
  success: 'border-$success-color text-$success-color',
} as const
```

- Use static maps, arrays, or class objects containing every complete candidate.
  Do not build utilities from fragments such as `` `text-${tone}-500` ``.
- Confirm that standalone `.ts`, generated templates, Markdown, and external
  packages are included by the configured content pipeline before moving class
  maps into them.
- When extraction cannot see a legitimate dynamic candidate, adjust content or
  add the smallest explicit safelist and verify the production output.
- Dynamic business classes defined in ordinary CSS are not UnoCSS candidates and
  do not need safelisting.

## Shortcuts And CSS Fallback

- Reuse layout shortcuts such as `flex-center` only when their complete behavior
  matches. A visually named shortcut can carry more contract than its name shows.
- Keep a one-off class combination in the component. Add a global shortcut after
  at least two stable consumers share the same meaningful abstraction.
- Do not create `@apply` aliases merely to hide a utility list. Prefer a component
  for behavioral/visual reuse or a semantic shortcut for stable utility reuse.
- Use scoped CSS when necessary; use `:deep()` or unscoped styles only when a
  component deliberately owns styles for slotted or third-party descendants.

## Delivery Check

- Confirm every utility is supported by the configured preset and transformer.
- Confirm dynamic candidates are statically extractable or minimally safelisted.
- Check mobile, desktop, hover, focus, disabled, light, and dark states relevant
  to the change.
- Run formatting, lint, typecheck, and a production build. Inspect the actual
  page when responsive layout or theme behavior changed.
