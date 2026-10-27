---
name: elixir-code-generation-contract
description: "Mandatory output contract for Elixir/Phoenix: Context boundary, Changesets, ExUnit tests. Trigger on new feature slices."
license: UNLICENSED
---

# elixir-code-generation-contract

## Objective

Standardize the format, OTP architecture, and minimum completeness of agent deliveries in Elixir and Phoenix applications.

## Compatibility (Cursor, Codex, Claude Code)

- Agent-vendor agnostic: treat as a mandatory delivery protocol.
- Prefer deterministic, reviewable outputs over conversational suggestions.
- Do not invent migrations, schemas, or dependencies silently without declaring assumptions.

## Apply When

- Creating or modifying a Phoenix API endpoint, Controller, or LiveView.
- Adding an Ecto Schema, Migration, Context function, or Oban worker.
- Implementing an OTP GenServer, Supervisor, or Task flow.
- Reviewing PRs or verifying whether generated Elixir code meets production quality.

## Mandatory Delivery Protocol

Every Elixir/Phoenix feature delivery MUST provide:

1. **Declared File Tree**: Relative paths of all created/modified files before code (`lib/my_app/domain/...`, `lib/my_app_web/controllers/...`, `test/...`).
2. **Boundary Contracts**:
   - Explicit Context boundary (`MyApp.Domain` module with public `@spec`).
   - `Ecto.Changeset` for data validation and cast limits.
   - Tagged tuples for outcomes: `{:ok, struct}` or `{:error, %Ecto.Changeset{} | atom()}`.
3. **Resilience & OTP Defaults**:
   - Explicit timeouts on `GenServer.call/3`, `Task.await/2`, and external HTTP calls.
   - Concurrency bounded via `Task.Supervisor.async_stream_nolink/4`.
   - Post-init work in `handle_continue/2` so supervisors never hang on startup.
4. **Security Baseline**:
   - Constant-time secret comparison with `Plug.Crypto.secure_compare/2`.
   - Never convert untrusted input with `String.to_atom/1` (use `String.to_existing_atom/1` or string keys).
   - Tenant scoping enforced inside query functions, not post-fetch in memory.
5. **Actionable Test Matrix (ExUnit)**:
   - Success path (`ConnCase` 200/201 or `LiveViewTest`).
   - Invalid changeset / validation error (`ConnCase` 422).
   - Unauthenticated guest denied (`ConnCase` 401).
   - Cross-tenant / unauthorized access denied (`ConnCase` 403).

## Mandatory Output Format

1. **Objective & Context**: Summary of the feature and bounded context.
2. **File Structure**: Tree of files to create or modify.
3. **Contracts & Schemas**: Ecto Schemas, Changesets, and typespecs.
4. **Context Implementation**: Business logic encapsulated in domain Context.
5. **Web / Presentation Layer**: Phoenix Controller, LiveView, or Plugs.
6. **Automated Tests**: ExUnit tests (`DataCase` and `ConnCase`).
7. **Verification Commands**: Exact commands to validate (`mix test`, `mix format --check-formatted`, `mix credo`).

## Additional Resources

- For checklist templates and complete context examples, read `reference.md`.
