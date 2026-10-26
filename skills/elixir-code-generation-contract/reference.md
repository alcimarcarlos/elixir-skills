# elixir-code-generation-contract Reference

## Standard Phoenix Slice Checklist

When implementing an endpoint or feature in Phoenix, follow this order:

- [ ] **1. Migration**: `priv/repo/migrations/...` with foreign keys and unique constraints.
- [ ] **2. Schema & Changeset**: `lib/my_app/domain/model.ex` with typed fields and validations.
- [ ] **3. Context Module**: `lib/my_app/domain.ex` exposing public functions returning `{:ok, _} | {:error, _}`.
- [ ] **4. Router**: `lib/my_app_web/router.ex` with proper pipeline and plugs.
- [ ] **5. Web Handler**: Controller/JSON view or LiveView handling user interaction.
- [ ] **6. ExUnit Tests**: `test/my_app/...` (DataCase) and `test/my_app_web/...` (ConnCase).

## Minimal ExUnit Test Skeleton

```elixir
defmodule MyAppWeb.OrderControllerTest do
  use MyAppWeb.ConnCase, async: true

  setup %{conn: conn} do
    user = MyApp.AccountsFixtures.user_fixture()
    {:ok, conn: log_in_user(conn, user), user: user}
  end

  test "renders 422 on invalid parameters", %{conn: conn} do
    conn = post(conn, ~p"/api/v1/orders", %{order: %{items: []}})
    assert json_response(conn, 422)["errors"] != %{}
  end

  test "creates order successfully", %{conn: conn} do
    conn = post(conn, ~p"/api/v1/orders", %{order: %{items: [%{item_id: 1, quantity: 2}]}})
    assert %{"id" => _id, "status" => "pending"} = json_response(conn, 201)["data"]
  end
end
```
