---
name: elixir-security-auth
description: "Elixir/Phoenix security: sessions, tokens, policies, tenant isolation, PII, rate limiting. Trigger on auth and security."
license: UNLICENSED
---

# Elixir Security and Auth

## Workflow

1. Identify the protected asset: account, tenant, admin action, API resource, file, token, PII, job or broadcast.
2. Trace the request/event from router or LiveView through authentication, authorization, validation, persistence, response, logs and async work.
3. Check direct object references and tenant boundaries before polishing code.
4. Validate both allowed and denied behavior with tests when possible.

## Agent Compatibility (Cursor, Codex, Claude Code)

- Use explicit checklists: authn, authz, input, output, logs, async, browser/session.
- Prefer Phoenix/Ecto primitives over bespoke security wrappers unless the project already has them.

## Boundary

- This is usually an additive skill, not a replacement for the implementation skill.
- Use it whenever identity, authorization, tenant isolation, uploads, secrets, PII, job args, PubSub payloads, LiveView assigns or API exposure are involved.
- Do not introduce a new auth library solely from this skill; follow the target project's existing auth stack.

## Defaults

- Authenticate in plugs/live sessions or API pipelines.
- Authorize per action and resource, not only by hiding UI.
- Scope queries by actor and tenant.
- Use changesets and whitelists for input.
- Keep CSRF enabled for browser forms.
- Use CORS narrowly for APIs that need cross-origin access.
- Validate uploads by MIME, extension, size, path, storage visibility and ownership.
- Never log secrets, tokens, passwords, raw PII payloads or payment data.
- Avoid sensitive data in LiveView assigns, PubSub messages, job args and telemetry metadata.

## Hardening Defaults

- Compare secrets, tokens and signatures in constant time with `Plug.Crypto.secure_compare/2`; never use `==` on secret material.
- Convert external strings with `String.to_existing_atom/1`, never `String.to_atom/1`, to prevent atom-table exhaustion (DoS).
- Hash passwords with a vetted KDF (Argon2/Bcrypt/Pbkdf2 via `comeonin`); never roll custom crypto or store reversible secrets.
- Rotate session/CSRF identifiers on login and privilege change to prevent session fixation.
- Rate-limit and throttle authentication, token, password-reset and other abuse-prone endpoints.
- Bound request/upload size, decompression and pagination to prevent resource-exhaustion (zip bombs, huge bodies, unbounded `limit`).
- Set timeouts and SSRF allowlists on outbound requests built from user-influenced URLs; block internal/metadata addresses.
- Keep secrets in runtime config/secret managers; load them in `config/runtime.exs`, never in committed config or compiled artifacts.

## Structured Threat Checks

- Broken access control: ownership, tenant boundaries, admin bypass paths.
- Injection: raw SQL fragments, unsafe dynamic order/filter, untrusted HTML, atom/code interpolation.
- Sensitive data exposure: JSON, logs, assigns, jobs, broadcasts, exception reports.
- SSRF/file upload: user-supplied URLs, storage visibility, signed URL scope and MIME enforcement.
- Resource exhaustion: unbounded lists, large uploads, atom creation, expensive regex on untrusted input.
- Timing attacks: non-constant-time comparison of tokens, signatures or password hashes.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
