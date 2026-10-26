# elixir-systematic-debugging Reference

## Diagnostic Commands

```bash
# Run a specific test at line number with full stacktrace
mix test test/my_app/orders_test.exs:42 --trace

# Drop into IEx debugger
iex -S mix test --trace

# Check compiler warnings
mix compile --warnings-as-errors
```

## Minimal Failing Test Template (ExUnit)

```elixir
defmodule MyApp.Orders.BugReproducerTest do
  use MyApp.DataCase, async: true

  test "reproduces edge case: calculates order total when item price is zero" do
    order = MyApp.OrdersFixtures.order_fixture()
    
    # Must fail before the bugfix is applied
    assert {:ok, %{total: 0}} = MyApp.Orders.calculate_total(order, [])
  end
end
```
