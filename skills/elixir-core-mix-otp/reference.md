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
