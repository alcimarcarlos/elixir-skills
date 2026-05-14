---
name: elixir-core-mix-otp
description: "Use for core Elixir application development, Mix projects, OTP architecture, Application callbacks, supervisors, GenServer, Agent, Task, Registry, DynamicSupervisor, PubSub-adjacent process design, runtime config, releases, code review, and architecture decisions."
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

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
