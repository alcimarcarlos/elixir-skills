# elixir-phoenix-liveview Reference

## Lifecycle Checklist

- What loads during disconnected mount?
- What requires `connected?(socket)`?
- Which params are handled by `handle_params/3`?
- Which events mutate state?
- Which state comes from PubSub/process messages?
- Which data should be streamed instead of assigned as a full list?

## Assigns

- Keep assigns small and serializable enough for LiveView diffs.
- Store IDs and lightweight view state; reload domain data through contexts when needed.
- Do not store secrets, tokens or raw sensitive payloads in assigns.
- Use temporary assigns or streams where large repeated data would otherwise grow memory.

## Forms

- Use changesets or `Phoenix.Component.to_form/2` according to project style.
- Validate on change only when the UX needs immediate feedback.
- Keep server-side validation authoritative.
- Test invalid and valid submit flows.

## Streams

- Use streams for large collections and incremental insert/update/delete.
- Ensure DOM containers and item IDs match LiveView stream requirements.
- Avoid mixing manual DOM IDs with unstable generated IDs.

## Testing

```bash
mix test test/<app>_web/live
mix test --failed
```

Use LiveViewTest helpers for:

- `live/2`
- `element/3`
- `has_element?/2`
- `form/3`
- `render_change/2`
- `render_submit/2`
- `render_click/1`
- redirect and patch assertions
