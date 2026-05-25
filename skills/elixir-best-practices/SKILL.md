---
name: elixir-best-practices
description: Elixir and Phoenix best practices for interactive development, debugging, testing, and routine code changes. Use when writing, reviewing, or troubleshooting Elixir application code, Mix tasks, Ecto code, or Phoenix features.
license: MIT
metadata:
  author: amami
  version: "1.1.0"
---

# Elixir Best Practices

Practical conventions for working in Elixir projects.

## When to Use

Use this skill when tasks include:

- Writing or refactoring Elixir application code
- Reviewing Phoenix, Ecto, or Mix-based project changes
- Debugging Elixir behavior in local development
- Running ad hoc code against an existing Mix project
- Validating Elixir changes with focused tests, formatting, linting, and static analysis

## Rule Categories

### 1. Interactive Development Workflow

- `iex-first-development`: Default to `iex -S mix` for Elixir development and examples; do not prefer `mix run --no-start -e '...'` unless non-interactive execution is required.

### 2. Code Change Discipline

- `small-composable-functions`: Prefer small functions with clear responsibilities over dense control flow.
- `pattern-matching-first`: Prefer pattern matching, function heads, and explicit data shapes over nested branching where it improves clarity.
- `pipelines-for-data-flow`: Use pipelines when they make sequential data transformation easier to read; do not force pipelines into branching-heavy logic.

### 3. Mix and Project Hygiene

- `format-before-finish`: Run `mix format` on touched code before finishing.
- `targeted-tests-first`: Run the smallest relevant `mix test` scope first, then widen only as needed.
- `task-over-script`: When behavior is already modeled by an existing Mix task, use the task instead of inventing ad hoc shell glue.

### 4. Code Quality Tooling

- `quality-deps-defaults`: Install `credo` and `dialyxir` with `only: [:dev, :test], runtime: false`. Install `ex_doc` with `only: :dev, runtime: false` unless the project intentionally builds docs in `MIX_ENV=test`.
- `ship-a-credo-config`: Add a minimal `.credo.exs` instead of relying on implicit defaults, so the repository has an explicit lint baseline.
- `quality-alias-standard`: Prefer a `mix quality` alias that runs `format --check-formatted`, `compile --warnings-as-errors`, `credo --strict`, and `dialyzer`.
- `dialyzer-mix-plt`: When the project contains Mix tasks, configure Dialyzer with `plt_add_apps: [:mix]` to avoid false positives around `Mix.Task` and `Mix.shell`.
- `quality-docs-visible`: Document the quality commands in the project README or equivalent contributor guide so local usage is obvious.
- `treat-existing-findings-as-backlog`: If adding quality tooling to an existing codebase surfaces many pre-existing findings, land the tooling first and treat the findings as follow-up work unless the task explicitly includes cleanup.

## How to Apply

1. Default to `iex -S mix` when the task involves manual code execution or examples.
2. Keep Elixir changes aligned with existing project style: small functions, readable pattern matching, and minimal incidental complexity.
3. If the repository lacks baseline quality tooling, add `Credo`, `Dialyxir`, and `ExDoc` using the standard dependency scopes above.
4. Add a minimal `.credo.exs` and a `mix quality` alias so the common local checks are easy to run consistently.
5. Run `mix format` after edits, but avoid expanding the task scope by committing unrelated formatting-only changes unless requested.
6. Validate with the narrowest relevant `mix test` command first, then run `mix compile --warnings-as-errors`; run `mix credo --strict` and `mix dialyzer` when the project has those tools configured.
7. If `mix dialyzer` reports issues, distinguish between configuration noise and real warnings before changing application code.

## Acceptance Checklist

- [ ] Interactive examples default to `iex -S mix` instead of `mix run --no-start -e '...'` where appropriate.
- [ ] Code changes follow existing Elixir style and avoid unnecessary control-flow complexity.
- [ ] Quality dependencies use the expected `only` and `runtime` settings.
- [ ] Repositories with quality tooling expose a `mix quality`-style entry point or an equivalent documented command set.
- [ ] Touched files pass `mix format`.
- [ ] Relevant tests are run at the smallest useful scope.
- [ ] `mix compile --warnings-as-errors` is run when practical.
- [ ] `mix credo --strict` and `mix dialyzer` are run when configured, or skipped with a clear reason.
