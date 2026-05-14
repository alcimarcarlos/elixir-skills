---
name: elixir-ash-framework
description: "Use for Ash Framework development, including resources, domains, actions, code interfaces, policies, validations, changes, calculations, aggregates, data layers, AshPostgres, AshPhoenix, AshJsonApi, AshGraphql, actor/tenant authorization, testing and architecture decisions."
---

# Elixir Ash Framework

## Workflow

1. Inspect Ash domains, resources, data layers, policies, code interfaces, extensions and tests.
2. Identify the domain operation as an action: read, create, update, destroy or generic.
3. Put business rules in resource actions, validations, changes, policies and calculations where Ash expects them.
4. Expose idiomatic calls through code interfaces for web/controllers/LiveViews.
5. Test action behavior, authorization and generated interfaces.

## Agent Compatibility (Cursor, Codex, Claude Code)

- State the resource/action model before implementation.
- Keep Ash decisions declarative and close to resources/domains.

## Boundary

- Use this skill only when the project already uses Ash or the task explicitly asks to adopt Ash.
- Use Ash resources/actions/policies/code interfaces as the domain API; do not create parallel Phoenix context APIs unless the project already has that convention.
- Use `elixir-ecto-data-performance` for migrations, AshPostgres data-layer performance, indexes or direct query work.
- Add `elixir-security-auth` whenever actor, tenant, policy or field authorization matters.

## Defaults

- Use actions as the primary interface to resource behavior.
- Use code interfaces for clean calls from Phoenix or other application modules.
- Pass `actor`, `tenant`, `context` or `scope` explicitly when authorization or tenancy matters.
- Use policies for authorization, including read filtering behavior.
- Prefer Ash extensions already present in the project over manual parallel implementations.
- Avoid bypassing Ash with direct Repo calls unless the project has a deliberate escape hatch.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
