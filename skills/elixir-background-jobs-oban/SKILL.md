---
name: elixir-background-jobs-oban
description: "Use for Oban background jobs, workers, queues, retries, scheduling, cron, uniqueness, idempotency, backoff, telemetry, plugins, job arguments, observability, and Oban testing."
license: UNLICENSED
---

# Elixir Background Jobs with Oban

## Workflow

1. Inspect `mix.exs`, Oban config, repo config, workers, migrations, telemetry and tests.
2. Identify the job contract: queue, args, idempotency key, retry behavior, uniqueness, scheduling and side effects.
3. Keep job args small, JSON-safe and free of secrets.
4. Make `perform/1` idempotent and safe to retry.
5. Test enqueue behavior and worker execution using the project's Oban testing mode.

## Agent Compatibility (Cursor, Codex, Claude Code)

- State retry and idempotency behavior explicitly.
- Keep background work Oban-native instead of inventing ad hoc process queues.

## Boundary

- Use this skill only when the project already uses Oban or the task explicitly asks to introduce durable jobs.
- Use `elixir-core-mix-otp` instead for short-lived supervised tasks, GenServers, in-memory queues or process coordination that does not require persistence/retries.
- Add `elixir-security-auth` when job args identify users, tenants, files, tokens, PII or authorization-sensitive resources.
- Add `elixir-ecto-data-performance` when the worker performs DB writes, transactions or query-heavy processing.

## Defaults

- Use Oban for durable background work when the project already depends on it.
- Use supervised Tasks only for bounded in-memory concurrency that does not need durability.
- Keep workers focused on one job type.
- Load records inside the worker from IDs, not full serialized structs.
- Return `:ok`, `{:ok, value}`, `{:cancel, reason}`, `{:snooze, seconds}` or errors according to project/Oban version style.
- Use uniqueness for duplicate prevention, but still make effects idempotent.
- Instrument or log with redacted metadata.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
