# Elixir Skill Selection Matrix

Use this matrix before loading multiple skills. The goal is to choose the smallest precise set and avoid conflicting guidance.

Canonical skill paths use the Codex/Claude-compatible format: `skills/<skill>/SKILL.md`. Cursor aliases may exist at `.cursor/skills/<skill>`, but they are not the source of truth.

| Task signal | Primary skill | Add when needed | Do not use when |
| --- | --- | --- | --- |
| Plain modules, Mix config, releases, process design, supervision | `elixir-core-mix-otp` | `elixir-testing-quality`, `elixir-security-auth` | The change is only a Phoenix template or only SQL/migration work |
| JSON endpoint, router, controller, plug, API error contract | `elixir-phoenix-api` | `elixir-ecto-data-performance`, `elixir-security-auth`, `elixir-testing-quality` | The screen is a LiveView event/rendering flow |
| Schema, changeset, query, migration, preload, transaction | `elixir-ecto-data-performance` | `elixir-security-auth`, `elixir-testing-quality` | The project uses Ash resources as the domain API and you are not changing low-level Ecto |
| HEEx, LiveView event, component, stream, upload, live navigation | `elixir-phoenix-liveview` | `elixir-ecto-data-performance`, `elixir-security-auth`, `elixir-testing-quality` | The route is only a stateless JSON/API endpoint |
| Durable job, queue, retry, cron, uniqueness, scheduled work | `elixir-background-jobs-oban` | `elixir-ecto-data-performance`, `elixir-security-auth`, `elixir-testing-quality` | The work is short-lived in-memory concurrency that does not need persistence |
| Tests, review, CI, Credo, Dialyxir, formatter | `elixir-testing-quality` | Any touched domain skill | The user only asks for conceptual explanation and no code/review |
| Auth, authorization, tenant, secrets, upload, PII | `elixir-security-auth` | The primary implementation skill | The task is purely internal and has no access/data exposure concerns |
| Ash resources, actions, policies, code interfaces | `elixir-ash-framework` | `elixir-security-auth`, `elixir-testing-quality` | The project does not depend on Ash |
| Nx tensors, defn, EXLA/Torchx, Axon, Bumblebee, inference | `elixir-nx-ml` | `elixir-core-mix-otp`, `elixir-testing-quality`, `elixir-security-auth` | The task is ordinary business logic or Ecto data work |

## Conflict Resolution

- Local project conventions beat this package.
- `mix.exs` beats assumptions.
- A project using Ash should not receive parallel Ecto context APIs unless the target codebase already mixes those layers.
- A project without Oban should not get Oban for a one-off async operation; use OTP primitives or the existing job library.
- A project without Nx should not get Nx for ordinary list/math utilities.
- A security skill can be additive but should not replace the implementation skill.
- A testing skill can be additive but should not introduce new test libraries unless already configured or explicitly requested.

## Minimum Skill Sets

- Phoenix JSON CRUD: `elixir-phoenix-api`, `elixir-ecto-data-performance`, `elixir-security-auth`, `elixir-testing-quality`.
- LiveView CRUD screen: `elixir-phoenix-liveview`, `elixir-ecto-data-performance`, `elixir-security-auth`, `elixir-testing-quality`.
- OTP worker/process: `elixir-core-mix-otp`, `elixir-testing-quality`.
- Oban worker: `elixir-background-jobs-oban`, `elixir-testing-quality`; add `elixir-security-auth` if args contain user/account/data references.
- Ash feature: `elixir-ash-framework`, `elixir-security-auth`, `elixir-testing-quality`; add Phoenix skill only for web exposure.
