---
name: elixir-phoenix-api
description: "Use for Phoenix API design and implementation, including routers, controllers, plugs, JSON responses, REST contracts, API versioning, pagination, error handling, authentication boundaries, OpenAPI-style contracts, async 202 workflows, and ConnCase tests."
license: UNLICENSED
---

# Elixir Phoenix API

## Workflow

1. Read existing `router.ex`, controllers, contexts, plugs, fallback controllers, JSON views/modules and tests.
2. Decide the contract before code: endpoint, method, auth, params/body, response status, JSON shape, errors, pagination and version behavior.
3. Put authentication/authorization at the boundary, but keep business rules in context/domain modules.
4. Serialize intentionally with JSON modules/views or explicit maps, not raw structs with associations.
5. Test success, validation failure, authorization failure, not found and pagination/version behavior when applicable.

## Agent Compatibility (Cursor, Codex, Claude Code)

- Describe the API contract in plain terms first.
- Keep API guidance Phoenix-native: router, plugs, controllers, contexts, fallback controllers and ConnCase.

## Boundary

- Use this skill for stateless HTTP behavior: JSON APIs, controllers, plugs, router pipelines, request/response contracts and API tests.
- Do not use this as the primary skill for HEEx, LiveView events, assigns, streams, live navigation or component state; use `elixir-phoenix-liveview`.
- Add `elixir-security-auth` whenever the endpoint depends on actor, token, session, tenant, role or ownership.
- Add `elixir-ecto-data-performance` when the endpoint changes schemas, changesets, queries, migrations or transaction behavior.

## API Defaults

- Use explicit route scopes, pipelines and named helpers/verified routes where available.
- Use contexts for domain behavior.
- Use `201 Created` for create, `202 Accepted` for queued/background work and `204 No Content` for successful deletes with no body.
- Prefer bounded pagination for lists.
- Use changesets or embedded schemas for external input validation.
- Use fallback controllers or consistent error translation when the project has that pattern.
- Avoid leaking Ecto structs, unloaded associations or exception internals.

## Security + Performance Defaults

- Enforce authz before data leaves the context.
- Scope queries by actor/tenant where applicable.
- Avoid N+1 and unbounded lists; preload intentionally and paginate.
- Never expose sensitive fields in JSON or errors.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
