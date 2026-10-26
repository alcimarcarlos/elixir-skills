# elixir-core-mix-otp Reference

## Mix and Project Inspection

- Check `mix.exs` for app name, version constraints, aliases, dependencies and preferred quality commands.
- Check `.formatter.exs` before changing formatting.
- Check `config/runtime.exs` before adding runtime environment reads.
- Check `lib/<app>/application.ex` before adding supervised processes.

## Process Choice

- Plain module: pure transformations, orchestration without state, adapters called by other modules.
- Agent: small state wrapper with simple operations.
- GenServer: stateful process, message handling, back-pressure or external resource coordination.
- Task: bounded async work whose result matters.
- Task.Supervisor: supervised concurrent tasks.
- DynamicSupervisor: children created/stopped at runtime.
- Registry: named process lookup or dispatch.
- GenStage/Broadway: back-pressure pipelines, ingestion or demand-driven work.

## Supervision

- Prefer `:one_for_one` for independent children.
- Use `:rest_for_one` when later children depend on earlier children.
- Use `:one_for_all` only when all children must restart together.
- Choose restart policy intentionally: `:permanent`, `:transient`, `:temporary`.
- Keep child args explicit and inspectable.

## Return Contracts

- Use `{:ok, value}` and `{:error, reason}` for recoverable business outcomes.
- Use `:ok` or `:error` for simple status.
- Raise for programmer errors, invalid invariants or unrecoverable failures.
- Avoid atom creation from untrusted input.

## Stability and Resilience

- Default `GenServer.call/3` timeout is 5s; set it explicitly per call based on the work, and handle `:timeout`/`:exit` at the caller.
- Set restart intensity intentionally: a tight `max_restarts`/`max_seconds` window surfaces persistent failures instead of masking them.
- Use `handle_continue/2` for post-init work so supervisors are not blocked on slow startup.
- Trap exits only when the process must clean up resources in `terminate/2`; remember `terminate/2` is not guaranteed on `:brutal_kill` or VM crash.
- Add circuit-breaker/timeout/retry-with-backoff around flaky external dependencies rather than letting a slow dependency exhaust the pool.
- Prefer `Task.Supervisor.async_stream_nolink/4` with bounded `max_concurrency` for fan-out so one failing task cannot crash the caller.

## Performance and Memory

- Reach for ETS (`:set`/`:ordered_set`, `read_concurrency: true`) for shared read-heavy caches; use `:persistent_term` for rarely-changing global config.
- Avoid a single GenServer as a serialization bottleneck on hot paths; partition with `PartitionSupervisor` or move state to ETS.
- Keep messages small: large binaries are reference-counted and shared, but copying big tuples/maps across processes is expensive.
- Watch for binary memory growth from many small sub-binaries; `:binary.copy/1` long-lived slices of large binaries when needed.
- Prefer `Stream` for large pipelines and `Enum` only when the full result is required.
- Use `:persistent_term`/compile-time data for lookups instead of recomputing on every call.

## Config

- Use `Application.compile_env/3` only for compile-time configuration.
- Use runtime config for deployment-dependent values.
- Keep secrets out of committed config.
- Prefer explicit config modules/functions when many callers need the same value.

## Quality Gates

```bash
mix compile --warnings-as-errors
mix test
mix test test/path/to/file_test.exs
mix format --check-formatted
mix credo
mix dialyzer
```
