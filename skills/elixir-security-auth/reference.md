# elixir-security-auth Reference

## Phoenix Auth Boundaries

- Router pipelines should make authentication expectations clear.
- Plugs should assign the current actor/account consistently.
- LiveViews should use live sessions/on_mount hooks according to project style.
- APIs should have explicit token/session handling and consistent 401/403 behavior.

## Authorization

- Check authorization before mutation and before returning sensitive data.
- Enforce tenant filters in queries, not only after fetching.
- Test same-tenant allow and cross-tenant deny.
- Avoid trusting route IDs or LiveView params without ownership checks.

## Input and Output

- Use changesets or explicit validators.
- Whitelist filter/sort fields.
- Escape user content through HEEx defaults; be careful with raw HTML.
- Do not expose internal exception messages in API errors.

## Secrets and Sensitive Data

- Keep secrets in runtime config/environment/secret manager, not committed config.
- Redact logs and inspect output.
- Avoid putting sensitive values into Oban args, PubSub messages, LiveView assigns or telemetry metadata.
- Prefer storing hashes/tokens according to framework/library guidance, not custom crypto.

## Tests

```bash
mix test
mix test test/path/to/security_or_auth_test.exs
```

Cover:

- unauthenticated
- authenticated but forbidden
- cross-tenant access
- invalid input
- sensitive fields absent from response/loggable payloads
