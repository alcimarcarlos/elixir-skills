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

## Crypto and Token Handling

- Use `Plug.Crypto.secure_compare/2` for any secret/token/signature comparison; plain `==` leaks length and timing.
- Use `Phoenix.Token`/`Plug.Crypto` (or the project's auth library) for signed/encrypted tokens with explicit `max_age`.
- Hash passwords with Argon2/Bcrypt/Pbkdf2; keep a dummy verify on unknown users to avoid user-enumeration via timing.
- Generate tokens/ids with `:crypto.strong_rand_bytes/1`, never `:rand`.
- Scope and expire signed URLs; never trust client-provided expiry or signature claims without verification.

## Abuse and Resource Limits

- Rate-limit auth, token, reset and write-heavy endpoints; apply per-account and per-IP limits where relevant.
- Cap `limit`/page size and reject unbounded list requests.
- Bound body size (`Plug.Parsers` `:length`), upload size and decompression ratios.
- Use `String.to_existing_atom/1` for external keys; whitelist dynamic filter/sort fields against a known set.
- Set connect/receive timeouts and SSRF allowlists on outbound HTTP derived from user input.

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
- rate-limit/abuse paths reject when exceeded
- token expiry/tamper rejection
- constant-time comparison used for secrets
