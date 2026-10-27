---
name: elixir-phoenix-liveview
description: "Phoenix LiveView: HEEx components, forms, streams, assigns, PubSub, LiveViewTest. Trigger on LiveView UI."
license: UNLICENSED
---

# Elixir Phoenix LiveView

## Workflow

1. Inspect router live routes, LiveViews, components, templates, contexts, PubSub usage and LiveView tests.
2. Identify lifecycle needs: mount, params, connected state, events, async work, streams, uploads or broadcasts.
3. Keep state minimal and derived data close to render.
4. Push business behavior into contexts/domain modules; keep LiveView responsible for interaction state.
5. Test with `Phoenix.LiveViewTest` for rendered state and user interactions.

## Agent Compatibility (Cursor, Codex, Claude Code)

- Describe expected user interaction states before implementation.
- Keep UI behavior server-rendered and Phoenix-native unless the project already has JS hooks for that interaction.

## Boundary

- Use this skill for stateful server-rendered UI: LiveView lifecycle, HEEx, function components, LiveComponents, assigns, streams, uploads, PubSub and LiveView tests.
- Do not use this as the primary skill for plain JSON endpoints or controller-only flows; use `elixir-phoenix-api`.
- Add `elixir-security-auth` for authenticated live sessions, role/tenant checks, uploads or sensitive assigns.
- Add `elixir-ecto-data-performance` when the LiveView changes persistence, queries, preloads or transactions.

## Defaults

- Use HEEx and function components for reusable markup.
- Use LiveComponents for stateful reusable UI only when component state/lifecycle is needed.
- Use streams for large or frequently changing collections.
- Use `assign_async`/async patterns only when supported by the project version and lifecycle.
- Avoid storing large collections or sensitive values in assigns.
- Use PubSub for cross-process updates and unsubscribe/cleanup according to lifecycle.
- Keep IDs stable for patches, streams and tests.
- Test both initial render and event-driven updates.

## Stability + Performance Defaults

- Each connected LiveView is a long-lived process; keep assigns small and use streams/`temporary_assigns` so memory does not grow per connection.
- Re-check authorization in `mount/3` and event handlers; client-pushed events and params are untrusted even after mount.
- Offload slow work to `assign_async`/`start_async` so the LiveView process and its message queue stay responsive.
- Debounce/throttle high-frequency events (`phx-debounce`/`phx-throttle`) to avoid flooding the process mailbox.
- Validate uploads (size, count, MIME, extension) with `allow_upload/3` constraints, not only client hints.
- Move heavy computation into contexts and cache derived data; avoid recomputing large structures on every render.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
