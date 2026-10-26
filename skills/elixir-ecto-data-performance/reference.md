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
- Use `Repo.stream/2` (inside a transaction) for exports and large scans.
- Pass `:timeout` for known-slow queries instead of relying on the default.
- Verify the plan with `EXPLAIN`/`Repo.explain/3` when a query is hot or suspected slow.

## Pools and Concurrency

- Size the pool to actual concurrent DB work; an oversized pool can overwhelm the database.
- Keep transactions short; never hold a connection across external HTTP/sleep/IO.
- Set `:queue_target`/`:queue_interval` awareness when checkout timeouts appear under load.
- Acquire locks in a consistent order across code paths to prevent deadlocks.

## Transactions

- Use `Repo.transaction/1` for small transactional blocks.
- Use `Ecto.Multi` when steps need named results, composability or testable structure.
- Keep external side effects outside DB transactions when possible; enqueue after commit if correctness depends on committed data.

## Migrations

- Add indexes for foreign keys and frequent filters.
- Use concurrent index creation patterns if the project supports them and the DB requires low-lock changes.
- Split risky large-table changes into additive phases.
- Keep data migrations idempotent when they may be rerun.
- Disable the migration transaction (`@disable_ddl_transaction true`) only for concurrent index builds, and backfill data in bounded batches.
- Add `NOT NULL`/constraints in phases on large tables (add nullable, backfill, validate, enforce) to avoid long locks.

## Quality Gates

```bash
mix test
mix test test/path/to/context_test.exs
mix ecto.migrate
mix ecto.rollback
mix format --check-formatted
```
