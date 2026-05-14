# elixir-ecto-data-performance Reference

## Schema Defaults

- Keep schemas focused on fields, associations, changesets and local query helpers.
- Use explicit field types and `timestamps()` conventions from the project.
- Avoid deriving JSON encoders that expose all fields.
- Use redaction for sensitive inspect output where supported by local style.

## Changesets

- `cast` only accepted fields.
- `validate_required` business-required input.
- Use `validate_format`, `validate_number`, `validate_length`, `validate_inclusion` as needed.
- Pair validations with database constraints when concurrent writes matter.
- Use `unsafe_validate_unique` only as UX help, not as the true guarantee.

## Queries

- Compose query functions for reusable filters.
- Use `select` when large columns are unnecessary.
- Use `preload` intentionally; avoid preloading everything by default.
- Keep dynamic filters whitelisted.
- Avoid `Repo.all` for unbounded user-facing lists.

## Transactions

- Use `Repo.transaction/1` for small transactional blocks.
- Use `Ecto.Multi` when steps need named results, composability or testable structure.
- Keep external side effects outside DB transactions when possible; enqueue after commit if correctness depends on committed data.

## Migrations

- Add indexes for foreign keys and frequent filters.
- Use concurrent index creation patterns if the project supports them and the DB requires low-lock changes.
- Split risky large-table changes into additive phases.
- Keep data migrations idempotent when they may be rerun.

## Quality Gates

```bash
mix test
mix test test/path/to/context_test.exs
mix ecto.migrate
mix ecto.rollback
mix format --check-formatted
```
