# Repository Guidelines

## Project Structure & Module Organization

This repository is a curated collection of Agent Skills. Each skill lives under
`skills/<skill-name>/` and must include a `SKILL.md`. Optional UI metadata lives
in `skills/<skill-name>/agents/openai.yaml`. Keep skill bodies concise and move
only genuinely reusable supporting material into optional `scripts/`,
`references/`, or `assets/` folders inside that skill.

Root-level configuration includes `package.json`, `eslint.config.mjs`,
`.oxfmtrc.jsonc`, and `.editorconfig`. The README lists public skills and should
be updated when skills are added, renamed, or moved.

## Build, Test, and Development Commands

- `pnpm format` runs `oxfmt` and rewrites formatted files.
- `pnpm format:check` verifies formatting without changing files.
- `pnpm lint` runs ESLint with `@ntnyq/eslint-config`.
- `pnpm prepare` installs Husky hooks.

There is no build step for the skills themselves. Validate changes with
`pnpm format:check` and `pnpm lint` before handing off.

## Coding Style & Naming Conventions

Use 2-space indentation, LF line endings, UTF-8, final newlines, and no trailing
whitespace. Markdown may preserve trailing whitespace when intentional.

Skill directory names use lowercase hyphen-case, for example
`typescript-style` or `project-structure`. `SKILL.md` frontmatter must include
`name` and `description`; descriptions should include practical trigger phrases,
including Chinese phrases when relevant. Keep `agents/openai.yaml` aligned with
the skill's purpose.

## Testing Guidelines

This repo currently has no unit test suite. Treat formatting, linting, and
frontmatter validity as the required checks. When adding scripts, test them by
running the script directly with representative inputs.

## Commit & Pull Request Guidelines

The history currently uses concise Conventional Commit style, such as
`chore: init commit`. Continue with short prefixes like `feat:`, `fix:`,
`docs:`, and `chore:`.

Pull requests should summarize changed skills, explain trigger or structure
changes, and note validation results. Include screenshots only when changing
visual assets or UI metadata that affects skill presentation.

## Agent-Specific Instructions

When running shell commands as an AI coding agent in this workspace, prefix
commands with `rtk`, for example `rtk pnpm lint` or `rtk git status`.
