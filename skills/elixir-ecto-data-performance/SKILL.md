---
name: elixir-ecto-data-performance
description: "Use for Ecto schemas, changesets, queries, associations, migrations, Repo usage, transactions, Ecto.Multi, preloads, N+1 prevention, indexes, database constraints, data migrations, large datasets, caching-adjacent data access, and performance review."
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

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
