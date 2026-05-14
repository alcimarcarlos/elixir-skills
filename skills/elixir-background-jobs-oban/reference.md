# elixir-background-jobs-oban Reference

## Worker Checklist

- Queue name and concurrency.
- Args shape and versioning.
- Retry/backoff expectations.
- Uniqueness period and fields.
- Idempotency guard.
- External side effects and compensation.
- Telemetry/logging metadata.

## Args

- Use string-keyed JSON-safe maps.
- Store IDs and primitive values.
- Do not store secrets, raw tokens, full structs or large blobs.
- Include a version key for long-lived scheduled jobs when payloads may evolve.

## Testing

- `:inline` mode executes jobs immediately and is simple for basic cases.
- `:manual` mode lets tests assert enqueued jobs and drain queues deliberately.
- Use `Oban.Testing.assert_enqueued/1`, `refute_enqueued/1`, `all_enqueued/1` and `Oban.drain_queue/1` when configured.

```bash
mix test test/path/to/worker_test.exs
mix test --failed
```

## Production Review

- Confirm Oban migration exists and is current.
- Confirm queues are configured for each environment.
- Confirm plugins/cron are not accidentally enabled in test.
- Confirm jobs are observable enough to debug failures without leaking sensitive data.
