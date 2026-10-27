---
name: elixir-core-mix-otp
description: "Elixir/OTP core: GenServer, supervisors, Tasks, Registry, runtime config. Trigger on OTP and Mix architecture."
license: UNLICENSED
---

# Elixir Core, Mix and OTP

## Workflow

1. Inspect `mix.exs`, `.formatter.exs`, `config/`, `lib/`, `test/`, application module and supervision tree.
2. Identify the slice: pure module, boundary API, supervised process, concurrent task, config, release/runtime concern or integration.
3. Follow the application's existing conventions first. Apply these defaults only where no local pattern is clear.
4. Design from the boundary inward: caller contract -> data transformation -> process/supervision needs -> failure behavior -> tests.
5. Validate with the smallest useful command set from `reference.md`.

## Agent Compatibility (Cursor, Codex, Claude Code)

- Keep instructions tool-agnostic: search/read/run rather than IDE-specific actions.
- Prefer small, reviewable diffs over framework-wide rewrites.
- Make supervision, failure modes and return contracts explicit.

## Boundary

- Use this skill as the base for ordinary Elixir, Mix, OTP, configuration, releases, process architecture and integrations.
- Do not default to GenServer for plain business logic; use modules/functions unless a process owns state or coordinates messages/resources.
- Use `elixir-background-jobs-oban` instead when work must be persisted, retried, scheduled or inspected as a durable job.
- Add Phoenix, Ecto, Ash, Nx, security or testing skills only when the touched code actually crosses those boundaries.

## Defaults

- Target the project's declared Elixir and Erlang/OTP versions.
- Prefer plain modules/functions for pure behavior.
- Use GenServer only when a process owns state, serializes access, coordinates resources or receives messages.
- Start long-running processes under a supervisor.
- Use `Task.Supervisor` for supervised async work.
- Use `DynamicSupervisor` for runtime-managed child processes.
- Use `Registry` for process lookup only when names or dispatch are needed.
- Add `@impl true` for callbacks and `@spec` for public APIs when the project uses specs.
- Prefer tagged tuples for recoverable outcomes.
- Do not hide errors with broad `rescue`; let supervisors restart crashable processes.

## Stability Defaults

- Set explicit timeouts on every `GenServer.call/3`, `Task.await/2` and external call; never rely on the implicit 5s default silently.
- Tune supervisor restart intensity (`max_restarts`, `max_seconds`) so a crash loop fails fast instead of hammering dependencies.
- Set `:shutdown` per child and trap exits only when cleanup is required; avoid `:brutal_kill` for processes holding resources.
- Bound concurrency with `Task.async_stream/3` (`max_concurrency`, `timeout`, `on_timeout: :kill_task`) instead of spawning unbounded tasks.
- Treat a growing process mailbox as a back-pressure failure; add demand control (GenStage/Broadway) or drop/reject load deliberately.
- Make `init/1` fast and non-blocking; do slow work in `handle_continue/2` so startup and supervision stay responsive.

## Performance Defaults

- Do not funnel read-heavy or hot-path state through a single GenServer; use ETS (`:read_concurrency`/`:write_concurrency`) or `:persistent_term` for near-static data.
- Avoid copying large binaries/terms between processes; pass references/IDs and read shared state from ETS.
- Never create atoms from untrusted input (`String.to_existing_atom/1`, not `String.to_atom/1`) to prevent atom-table exhaustion and VM crashes.
- Prefer streaming/lazy enumerables over building large intermediate lists in memory.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
