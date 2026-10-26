---
name: elixir-zero-downtime-migrations
description: "Zero-downtime database migration strategies in Elixir and Ecto (PostgreSQL): Expand/Contract pattern, concurrent non-blocking indexes, safe schema transitions, partitioned backfills, and table lock prevention."
license: UNLICENSED
---

# Elixir / Ecto Zero-Downtime Migrations

## Objective

Prevent database connection saturation and exclusive table locks when migrating schemas in production Elixir applications powered by Ecto and PostgreSQL.

## Compatibility (Cursor, Codex, Claude Code)

- Tool-agnostic playbook: applies to PostgreSQL via Ecto.
- Always isolate DDL statements from data transformations.
- Never add blocking non-nullable columns without defaults on populated tables.

## Golden Rules

1. **Concurrent Indexes with `@disable_ddl_transaction true`**:
   - Creating an index locks out table writes by default.
   - Set `@disable_ddl_transaction true` and use `create_if_not_exists index(..., concurrently: true)` so traffic is not blocked.
2. **Never Run Massive Backfills in `change/0` or `up/0`**:
   - Never run `Repo.update_all` directly inside an Ecto migration.
   - Use partitioned Oban background jobs or Mix tasks with streaming queries (`Repo.stream`) to backfill records in small, isolated batches.
3. **Phased Column Renames and Removals (Expand & Contract)**:
   - **Phase 1 (Expand)**: Add the new column as nullable; deploy code that writes to both columns.
   - **Phase 2 (Backfill)**: Backfill data in the background using Oban.
   - **Phase 3 (Switch)**: Deploy code that queries and updates only the new column. Drop the old field from the Ecto schema struct.
   - **Phase 4 (Contract)**: Drop the old column in a separate, isolated migration.

## Safe Migration Checklist

- [ ] Is concurrent index creation using `@disable_ddl_transaction true`?
- [ ] Are new columns `null: true` or instant defaults?
- [ ] Are foreign keys validated without long table locks?
- [ ] Is the migration reversible?

## Additional Resources

- Read `reference.md` for Ecto migration templates and concurrent index patterns.
