# Elixir Guidelines and Research Notes

## Source Baseline

The skills were curated from:

- Elixir official documentation and HexDocs: Mix, OTP, GenServer, Supervisor, ExUnit and release/runtime conventions.
- Phoenix and Phoenix LiveView HexDocs: routers, controllers, plugs, components, HEEx, LiveView lifecycle, streams and tests.
- Ecto and Ecto SQL HexDocs: schemas, changesets, queries, migrations, associations, transactions and preloads.
- Oban HexDocs: queue configuration, workers, persistence, retries and testing modes.
- Ash HexDocs: resources, actions, policies, code interfaces and actor/tenant authorization options.
- Nx HexDocs: tensors, `defn`, automatic differentiation, broadcasting and numerical backends.
- Credo and Dialyxir HexDocs for quality tooling.
- GitHub/community references such as `h4cc/awesome-elixir`, `phoenixframework/phoenix_live_view`, Cursor/Codex skill examples, and Phoenix/Elixir agent packs such as PhxAgents.

## General Defaults

- Treat `mix.exs` as the contract for versions, dependencies, aliases and quality commands.
- Confirm optional libraries in `mix.exs` before applying optional-stack guidance. Ash, Oban, Nx, Mox, Credo and Dialyxir are not defaults for every Elixir project.
- Prefer the app's existing context and folder structure.
- Keep web code at the boundary and business behavior in contexts/domain modules.
- Prefer immutable data transformations and pattern matching over mutable-style flow.
- Use `with` for linear failure pipelines when each step returns tagged tuples.
- Avoid rescuing exceptions for ordinary control flow.
- Prefer explicit return contracts: `{:ok, value}`, `{:error, reason}`, `:ok`, `:error`.
- Add `@impl true` for behaviour callbacks.
- Add `@spec` for public APIs and complex private helpers when Dialyzer is used.
- Use `Application.compile_env/3` for compile-time config and `Application.get_env/3` or runtime config for runtime variability.

## Architecture Defaults

- Use OTP supervision for long-running processes, connection clients, caches, registries and background workers.
- Do not start unsupervised processes for durable work.
- Keep GenServers responsible for process state and coordination, not as a default object substitute.
- Use Tasks for bounded async work and `Task.Supervisor` for supervised concurrent tasks.
- Use `Registry`, `DynamicSupervisor`, `PartitionSupervisor` or PubSub only when their operational semantics are actually needed.
- Use Oban or the project's existing durable job library for retryable, scheduled or auditable background work.

## Phoenix Defaults

- Use contexts as the web boundary for domain behavior.
- Controllers and LiveViews should orchestrate input, authorization, context calls and response/state updates.
- Keep HEEx templates declarative; move complex formatting or branching into components/helpers.
- Use plugs for cross-cutting request behavior.
- Use Phoenix verified routes where available.

## Ecto Defaults

- Use changesets for external input.
- Keep schemas focused on fields, associations, changesets and local query helpers.
- Use `Ecto.Multi` for multi-write business invariants.
- Preload intentionally. Avoid hidden N+1 in views, JSON encoders and LiveViews.
- Add indexes for lookup, join, uniqueness and ordering paths.
- In Ash projects, prefer Ash resources/actions/policies as the domain interface and reserve direct Ecto work for data layer, migration and query-level changes that the project already exposes.

## Quality Defaults

- Run targeted tests first.
- Keep async tests enabled only when shared state, DB sandbox and process messages are safe.
- Prefer contract and behavior assertions over implementation details.
- Use Mox or local behaviours for external services.
- Use Credo as style/design feedback, and Dialyxir for type/spec regressions when configured.

## Security Defaults

- Authenticate at the boundary.
- Authorize per action and per resource, including tenant ownership.
- Do not leak internals through errors, JSON, assigns, broadcasts, telemetry metadata or logs.
- Validate uploads by MIME, size, extension expectations, storage path and authorization.
- Treat job args, PubSub payloads and LiveView assigns as possible sensitive-data surfaces.
