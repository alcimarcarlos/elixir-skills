---
name: elixir-phoenix-liveview
description: "Use for Phoenix LiveView development, HEEx templates, function components, LiveComponents, forms, streams, assigns, PubSub updates, uploads, live navigation, accessibility, realtime UX, and LiveView tests."
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

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
