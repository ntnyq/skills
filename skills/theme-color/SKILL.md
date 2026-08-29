---
name: theme-color
description: Design, migrate, normalize, or review frontend colors and theme tokens across CSS, SCSS, Vue styles, charts, Canvas, and SVG. Use for dark mode, semantic color variables, hard-coded color cleanup, alpha/hex normalization, contrast checks, or Chinese prompts such as "颜色规范", "主题变量", "暗色模式", and "整理颜色字面量".
---

# Theme Color

Use this skill to preserve color meaning across light/dark themes and rendering
boundaries. Inspect the project's existing token definitions and computed theme
values before introducing names or replacing literals.

**Core constraint:** A color refactor must preserve rendered channels, alpha,
theme semantics, selectors, and component behavior unless a visual redesign is
explicitly requested.

## Workflow

- [ ] Locate global and local color tokens, theme selectors, component styles,
      utility-framework theme config, and script-side palettes.
- [ ] Classify each color as semantic UI color, fixed brand/decorative color,
      status color, data-visualization palette, or rendering-boundary value.
- [ ] Reuse or add the narrowest correct token; normalize literals only when the
      task includes formatting or migration.
- [ ] Compare computed light and dark values and inspect representative pages in
      both themes.

## Token Selection

Use this order:

1. Existing semantic tokens for page surfaces, text, borders, interaction,
   focus, disabled states, and success/warning/danger feedback.
2. Existing fixed scales for brand marks, decoration, legends, and colors that
   must remain identical across themes.
3. Existing RGB-channel or alpha-capable tokens for translucent backgrounds,
   borders, shadows, and glows.
4. A module-local semantic token when the global system cannot express a stable
   domain meaning.
5. A literal only when the value is intentionally one-off or cannot cross the
   relevant rendering boundary as a CSS variable.

- Do not replace a theme surface with a fixed white/black/palette value merely
  because it looks correct in one theme.
- Distinguish theme-aware inverse text from text that must remain permanently
  white or black on a fixed brand surface.
- Keep status semantics separate from decorative palettes. Purple or cyan is not
  a substitute for the application's success, warning, or danger contract.
- Reuse the project's existing glass, overlay, border, and shadow token family
  rather than mixing parallel naming systems within one component.

## Fixed And Script-Side Colors

- Keep fixed scales unchanged across theme selectors. If a nominally fixed token
  changes in dark mode, treat it as semantic until the token system is migrated.
- CSS variables may not work inside ECharts options, Canvas operations, SVG data,
  mock payloads, or third-party SDK configuration. Read computed tokens at the
  rendering boundary when live theme updates are required, or retain a clearly
  named script palette.
- Do not mechanically replace colors inside URLs, data URLs, SVG `url(#id)`
  references, or third-party static assets.
- Ensure `var(--token, fallback)` uses a fallback with the same meaning and
  expected value. Prefer defining a required missing token over hiding the
  problem with an unrelated fallback.

## Literal Normalization

When the task explicitly includes color-literal normalization:

- Preserve RGB channels and alpha exactly apart from the documented rounding
  policy. Do not change token names, selectors, declarations, or style structure.
- Write opaque hex in lowercase. Preserve valid short hex such as `#fff`; do not
  expand it only for consistency.
- Use `rgba(...)` for statically convertible alpha colors in this house style.
  Round alpha to at most three decimals and remove meaningless trailing zeros.
- Treat an alpha of 1 as opaque. Leave dynamic channels or dynamic alpha in the
  original form when an exact static conversion is impossible.
- Preserve CSS keywords such as `transparent`, `white`, and `black` unless the
  task is a semantic-token migration rather than a format-only cleanup.
- In a Vue SFC color-format task, limit edits to the style block unless the user
  also requested template, script, or palette changes.

## Styling Boundaries

- Prefer the project's configured utility framework for local layout and simple
  color/state application. Use CSS/SCSS for complex selectors, keyframes,
  third-party overrides, or shared cross-node behavior.
- Keep module-only colors local with a meaningful prefix. Promote them globally
  only after their semantics are stable across modules.
- When a color token changes meaning rather than only representation, treat it as
  a visual change and verify accessibility contrast and affected components.

## Anti-Patterns

- Replacing semantic backgrounds or text with a fixed palette value.
- Creating a second token with the same meaning because its current value differs.
- Changing hex length, RGB channels, alpha, or theme ownership during a
  format-only refactor.
- Assuming CSS variables work in every chart, canvas, SVG, or SDK boundary.
- Defining theme-specific overrides for a color that is supposed to remain fixed.

## Delivery Check

- Compare computed colors in light and dark themes, including hover, active,
  focus, disabled, status, overlay, and selected states.
- Inspect at least one representative light page and dark page for contrast,
  borders, shadows, and status communication.
- Search changed files for remaining equivalent literals, undefined variables,
  incompatible fallbacks, and script-side consumers.
- Run formatting, lint, typecheck, build, and the project's visual checks relevant
  to the changed styles.
