# elixir-phoenix-api Reference

## Contract Checklist

- Method and path.
- Pipeline and auth requirements.
- Request params/body and validation.
- Success status and JSON shape.
- Error statuses and JSON shape.
- Pagination and sorting rules.
- Side effects: jobs, emails, broadcasts, audit logs.

## Controller Defaults

- Keep controllers thin: parse/validate, authorize, call context, translate response.
- Prefer `action_fallback` when the project already maps tagged errors centrally.
- Avoid direct Repo calls from controllers unless the project is intentionally small and already does this.
- Avoid returning structs with fields that should remain private.

## Params and Validation

- Use changesets for data persisted through Ecto.
- Use embedded schemas or dedicated validator modules for non-persistent payloads.
- Whitelist sortable/filterable fields.
- Never pass user-provided field names into raw SQL fragments.

## Responses

- Keep JSON shapes stable and documented in tests.
- Use ISO8601 timestamps.
- Avoid atom keys in external JSON contracts unless the project consistently encodes them.
- Prefer explicit envelope/pagination metadata if the project already has an API style.

## Tests

```bash
mix test test/<app>_web/controllers
mix test --failed
mix phx.routes
```

Test:

- happy path
- invalid params
- unauthorized/forbidden
- not found
- tenant/ownership boundary
- JSON shape and pagination metadata
