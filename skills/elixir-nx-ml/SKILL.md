---
name: elixir-nx-ml
description: "Use for Numerical Elixir work with Nx, tensors, defn, EXLA, Torchx, Axon, Bumblebee, Scholar, Explorer-adjacent data preparation, embeddings, model inference, numerical pipelines, performance review and tests."
---

# Elixir Nx and ML

## Workflow

1. Inspect `mix.exs` for Nx, EXLA, Torchx, Axon, Bumblebee, Scholar, Explorer and serving dependencies.
2. Identify the numerical contract: input shape, type, backend, batching, model, output shape and error handling.
3. Keep tensor transformations explicit and tested with small deterministic examples.
4. Use `defn` where tensor-aware compilation/autodiff matters.
5. Validate shape/type behavior, performance-sensitive paths and fallback behavior.

## Agent Compatibility (Cursor, Codex, Claude Code)

- State tensor shapes and dtypes in plain terms.
- Prefer library-native primitives over hand-written numerical loops.

## Boundary

- Use this skill only when the project already uses Nx/EXLA/Torchx/Axon/Bumblebee or the task explicitly asks for Numerical Elixir or ML.
- Do not use Nx for ordinary business calculations, list transformations or Ecto aggregate queries.
- Add `elixir-security-auth` when inputs, prompts, embeddings, images or model outputs may contain sensitive data.

## Defaults

- Use Nx tensors for numerical data.
- Use `defn` for differentiable or compiled numerical functions.
- Choose backend deliberately: default, EXLA, Torchx or project-specific serving.
- Keep model loading and serving lifecycle explicit and supervised when long-running.
- Avoid silently converting huge datasets into memory-heavy tensors.
- Test with tiny fixtures and tolerance-based assertions for floats.

## Reference

Read `reference.md` for detailed patterns, checklists, and examples for this skill.
