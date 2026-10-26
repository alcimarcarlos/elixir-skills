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

## Stability + Performance Defaults

- Bound `max_attempts` and use capped exponential backoff so failing jobs do not retry forever or stampede dependencies.
- Set per-queue concurrency limits matched to downstream capacity (DB pool, external API), not an arbitrary high number.
- Enforce a job timeout so a stuck worker is reclaimed instead of holding a slot indefinitely.
- Make `perform/1` idempotent and guard against duplicate execution (uniqueness + an idempotency key/state check).
- Keep args small (IDs and primitives); load fresh records inside the worker so retries use current state.
- Enable pruning/retention and route exhausted jobs to a discard/dead-letter path with observability.
- Keep DB transactions short inside workers; do external I/O outside the transaction and after commit.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
