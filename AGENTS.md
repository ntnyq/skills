# Repository Guidelines

## Project Structure & Module Organization

This repository is a pnpm-managed collection of agent skills. Each skill lives in
`skills/<skill-name>/` and uses a required `SKILL.md` as the main instruction
file. Agent-specific adapters belong under `skills/<skill-name>/agents/`; for
example, `skills/vue-app/agents/openai.yaml`. Root files such as
`package.json`, `eslint.config.mjs`, `.oxfmtrc.jsonc`, and `pnpm-workspace.yaml`
define shared tooling and formatting.

## Build, Test, and Development Commands

Install dependencies with `pnpm install`. Use `pnpm lint` to run ESLint across
the repository, `pnpm format:check` to verify formatting, and `pnpm format` to
apply oxfmt formatting. The `prepare` script installs Husky hooks after
dependency installation. There is no build output for skills; review the
Markdown and YAML artifacts directly.

## Coding Style & Naming Conventions

Use 2-space indentation, LF line endings, UTF-8 text, and final newlines as
defined in `.editorconfig`. Skill directory names should be kebab-case, such as
`typescript-style` or `project-structure`. Keep skill instructions direct,
actionable, and scoped to the skill. For JavaScript, TypeScript, JSON-like
config, Markdown, and YAML, rely on oxfmt plus the shared `@ntnyq/eslint-config`
rules; do not hand-format around those tools.

## Testing Guidelines

This project currently has no dedicated automated test suite. Treat
`pnpm lint` and `pnpm format:check` as the required validation before opening a
pull request. When adding scripts or executable examples, include a clear manual
verification note in the PR description and keep fixtures under an ignored or
clearly named fixture directory if they are needed.

## Commit & Pull Request Guidelines

Recent history uses Conventional Commit-style prefixes, including `feat:`,
`docs:`, and `chore:`. Keep commit subjects short and imperative, for example
`docs: add vue testing skill notes`. Pull requests should describe the changed
skills, explain why the update is useful, list validation commands run, and link
related issues when available. Include screenshots only for documentation or
rendering changes where visual output matters.

## Agent-Specific Instructions

For automated work in this local Codex environment, prefix shell commands with
`rtk`, for example `rtk pnpm lint` or `rtk git status`.
