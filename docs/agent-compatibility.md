# Agent Compatibility (Cursor, Codex, Claude Code)

This repository contains agent skills intended to be usable across different coding agents.

## Compatibility Principles

- Keep `skills/<skill>/SKILL.md` as the canonical portable format for Codex/Claude.
- Keep `.cursor/skills/<skill>` as a symlink/alias only when Cursor compatibility is needed.
- Edit canonical files under `skills/`, not aliases under `.cursor/skills/`.
- Prefer tool-agnostic language: "search the codebase", "read the file", "run tests".
- Keep workflows deterministic and executable without proprietary IDE features.
- Prefer small, composable steps over large refactors.
- Inspect local conventions first: `mix.exs`, `config/`, `lib/`, `test/`, `router.ex`, contexts, schemas and supervision tree.
- Make validation, authorization, supervision, data consistency and performance explicit.

## Baseline Quality Gates

Run the smallest relevant set of commands that exist in the target project:

```bash
mix test
mix test test/path/to/file_test.exs
mix test --failed
mix format --check-formatted
mix credo
mix dialyzer
mix compile --warnings-as-errors
mix ecto.migrate
mix ecto.rollback
mix phx.routes
```

## Output Expectations

When implementing changes, prefer outputs that are easy for any agent to verify:

- changed behavior and why it changed
- tests added or updated for risky paths
- supervision/failure behavior for OTP or jobs
- authorization and sensitive-data notes
- performance notes for hot paths, queries, streams, preloads and background jobs
