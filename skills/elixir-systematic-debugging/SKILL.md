---
name: elixir-systematic-debugging
description: "Systematic root-cause debugging protocol for Elixir and Phoenix: prohibits guessing, enforces minimal failing ExUnit test reproducers, single-variable fixes, and non-regression verification."
license: UNLICENSED
---

# Elixir / Phoenix Systematic Debugging

## Objective

Eliminate guesswork ("shotgun debugging") by enforcing a strict 4-step scientific debugging method for triaging and resolving defects in Elixir, Phoenix, and LiveView codebases.

## Compatibility (Cursor, Codex, Claude Code)

- Strict operational gate: agents must not alter production source code until a minimal failing ExUnit test reproducer is written.
- Always isolate the root cause before implementing the fix.

## Mandatory 4-Step Protocol

### Step 1: Formulate Root-Cause Hypothesis
- Inspect the stack trace, error logs, and exception metadata.
- Formulate an explicit hypothesis: *"The bug occurs because module X handles pattern Y without a fallback match, triggering a FunctionClauseError."*

### Step 2: Create Minimal Failing Reproducer
- Create or add a test in `test/...` that directly triggers the reported scenario.
- Run the targeted test:
  ```bash
  mix test test/my_app/bug_reproducer_test.exs
  ```
- **Verification Gate**: The test MUST fail with the exact reported error. If it passes, the hypothesis is wrong or incomplete.

### Step 3: Surgical Fix (Single Variable)
- Make the smallest possible change to resolve the root cause.
- Follow the **Minimal Diff Mandate**: never rewrite unaffected functions, never modify unrelated supervision trees.

### Step 4: Non-Regression Verification
- Re-run the reproducer test to verify it passes:
  ```bash
  mix test test/my_app/bug_reproducer_test.exs
  ```
- Run the full test suite and formatter:
  ```bash
  mix test
  mix format --check-formatted
  ```

## Anti-Patterns (Strictly Forbidden)

- **Shotgun Debugging**: Guessing changes randomly across multiple modules.
- **Error Silencing**: Wrapping code in catch-all `rescue _ -> nil`.
- **Modifying Assertions**: Changing a test assertion to match incorrect output instead of fixing the root cause.

## Additional Resources

- Read `reference.md` for triage commands and reproducer templates.
