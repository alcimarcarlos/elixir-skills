---
name: elixir-ecto-data-performance
description: "Use for Ecto schemas, changesets, queries, associations, migrations, Repo usage, transactions, Ecto.Multi, preloads, N+1 prevention, indexes, database constraints, data migrations, large datasets, caching-adjacent data access, and performance review."
license: UNLICENSED
---

# Elixir Ecto Data and Performance

## Workflow

1. Inspect schemas, migrations, context modules, queries, tests, repo config and database adapter.
2. Identify the data invariant: validation, persistence, relationship, transaction, uniqueness, ownership, ordering or volume.
3. Model external input with changesets and database truth with constraints/indexes.
4. Make query shape explicit: filters, joins, preloads, pagination, locks and selected fields.
5. Validate with targeted tests, migration checks and query/performance review.

## Agent Compatibility (Cursor, Codex, Claude Code)

- State the data invariant before writing code.
- Keep persistence decisions Ecto-native: changesets, constraints, queries, Repo and `Ecto.Multi`.

## Boundary

- Use this skill for direct Ecto work: schemas, changesets, migrations, queries, Repo, constraints, preloads and `Ecto.Multi`.
- In Ash projects, use `elixir-ash-framework` as the primary skill for domain behavior modeled as resources/actions/policies. Use this skill only for low-level Ecto, AshPostgres data layer, migrations or explicit query/performance work.
- Add `elixir-security-auth` when queries must enforce actor, tenant, ownership or field exposure.
- Add `elixir-testing-quality` for any persistence behavior that can regress.

## Defaults

- Use changesets for external input.
- Use database constraints for uniqueness and foreign-key guarantees.
- Use `Ecto.Multi` for multi-write invariants.
- Use explicit preloads for relationships consumed by views, APIs or LiveViews.
- Avoid hidden queries inside loops and render paths.
- Add indexes for frequent `where`, `join`, `order_by`, foreign keys and unique lookups.
- Prefer cursor/keyset style pagination for large ordered datasets where UX allows it.
- Keep migrations reversible unless a one-way data operation is intentional and documented.

## Performance Defaults

- `select` only needed columns on hot paths; avoid loading large text/JSON/binary columns you do not use.
- Process large result sets with `Repo.stream/2` inside a transaction instead of `Repo.all`.
- Batch inserts/updates with `Repo.insert_all/3`/`update_all/3` rather than per-row writes in a loop.
- Preload with separate queries or joins deliberately; never trigger lazy association loads inside render/serialize loops (N+1).
- Set explicit query/transaction timeouts and size the connection pool to real concurrency, not an arbitrary large number.
- Keep transactions short; do not perform HTTP calls, sleeps or external I/O while holding a DB connection.

## Stability Defaults

- Enforce invariants with DB constraints (unique, FK, check, not-null), not only changeset validations, to survive races.
- Acquire row locks (`lock: "FOR UPDATE"`) in a consistent order to avoid deadlocks on concurrent updates.
- Run heavy/locking DDL with safe-migration patterns (concurrent indexes, additive phases, backfill in batches) to avoid long table locks.
- Make data migrations idempotent and resumable; never assume a one-shot run on large tables.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
