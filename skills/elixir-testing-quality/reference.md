# elixir-testing-quality Reference

## ExUnit

- Use descriptive test names.
- Prefer pattern matching assertions for return contracts.
- Use `setup` for reusable fixture state.
- Keep async true only when no shared process/global state can leak.
- Use `assert_receive` with tight, meaningful messages.
- Avoid arbitrary sleeps; prefer message synchronization or polling helpers already present in the project.

## Phoenix Tests

- ConnCase for HTTP/controller/API behavior.
- DataCase for contexts and database behavior.
- LiveViewTest for rendered state and events.
- Assert status, redirects, JSON shape, DB state and side effects.

## External Boundaries

- Define behaviours for replaceable clients.
- Use Mox expectations for contract-level behavior.
- Use Bypass when the HTTP layer itself matters.
- Do not mock modules inside the same domain unless the project already follows that style.

## Quality Tools

```bash
mix test
mix test test/path/to/file_test.exs
mix test --failed
mix format --check-formatted
mix credo
mix dialyzer
mix compile --warnings-as-errors
```

## Review Checklist

- Missing authz/tenant tests.
- Missing validation or constraint tests.
- Flaky async/process tests.
- Hidden N+1 in render/serialization paths.
- Job retry/idempotency untested.
- LiveView tests that assert text only while missing event behavior.
