# elixir-ash-framework Reference

## Resource Checklist

- Domain placement.
- Data layer.
- Attributes and identities.
- Relationships.
- Actions and primary actions.
- Validations and changes.
- Calculations and aggregates.
- Policies and field policies.
- Code interfaces.
- Notifications and side effects.

## Actions

- Model each domain operation as a named action.
- Use action arguments for explicit input.
- Prefer validations/changes over ad hoc web-layer logic.
- Use transactions according to action semantics and data layer behavior.

## Code Interfaces

- Define resource/domain functions for web-facing calls.
- Prefer `get_by` style interfaces for common single-record lookups.
- Use generated bang/non-bang functions according to caller needs.
- Pass `actor`, `tenant`, `load`, `query`, `authorize?` and pagination options deliberately.

## Policies

- Treat read authorization as filtering by default unless the project chooses error behavior.
- Use actor-aware policies for create/update/destroy.
- Test unauthorized reads and writes.
- Keep authorization checks reusable when repeated across resources.

## Tests

```bash
mix test test/path/to/ash_resource_test.exs
mix test --failed
mix format --check-formatted
```
