# elixir-nx-ml Reference

## Tensor Checklist

- Input shape.
- Output shape.
- dtype.
- named axes if used.
- backend.
- batch size.
- memory expectations.
- deterministic seed if randomness is used.

## `defn`

- Use tensor-aware operations inside `defn`.
- Keep non-numerical branching outside compiled numerical functions when possible.
- Be explicit about shapes and broadcasting.
- Use tolerance-based comparisons for floating point tests.

## Serving and Models

- Load models once when expensive.
- Supervise long-running serving processes.
- Keep preprocessing and postprocessing contracts tested.
- Avoid logging raw prompts, images, embeddings or sensitive input values.

## Tests

```bash
mix test test/path/to/numerical_test.exs
mix format --check-formatted
```

Use:

- small deterministic tensors
- shape assertions
- dtype assertions
- tolerance assertions for floats
- backend-independent tests where possible
