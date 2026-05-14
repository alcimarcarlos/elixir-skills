---
name: elixir-testing-quality
description: "Use for Elixir test writing and quality review, including ExUnit, ConnCase, DataCase, LiveViewTest, Mox, Bypass, StreamData, factories/fixtures, async tests, formatter, Credo, Dialyxir, compile warnings, CI readiness and code review."
---

# Elixir Testing and Quality

## Workflow

1. Detect test style from `test/`, `test_helper.exs`, case templates, factories/fixtures and `mix.exs` aliases.
2. Follow existing test style.
3. Cover behavior that can regress, not every line.
4. Use fakes/mocks at external boundaries and real modules for internal behavior when practical.
5. Run targeted tests first, then formatter/static analysis if available.

## Agent Compatibility (Cursor, Codex, Claude Code)

- Keep tests deterministic and isolated.
- Prefer assertions on contracts, state changes and messages over private implementation details.

## Boundary

- This is usually an additive skill for implementation, review and release-readiness work.
- Do not introduce Mox, Bypass, StreamData, Credo or Dialyxir unless the project already uses them or the user explicitly asks to add them.
- Prefer the project's existing case templates, fixtures/factories and aliases before adding new test infrastructure.

## Defaults

- Use ExUnit and project case templates.
- Use async tests only when DB sandbox, process messages and shared global state are safe.
- Use Mox for behaviours/external services when configured.
- Use Bypass or local HTTP fakes for external HTTP.
- Use StreamData for property tests only when the invariant benefits from generated inputs.
- For code review, lead with behavioral bugs, security risks, data consistency and missing tests.
- Run formatter, Credo and Dialyzer when configured.

## Quality Gates (recommended order)

1. Run the smallest relevant test subset.
2. Run formatting check.
3. Run compile warnings/static analysis.
4. Run broader suite only when shared surfaces were touched.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
