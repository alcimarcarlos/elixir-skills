# elixir-zero-downtime-migrations Reference

## Concurrent Index Creation in Ecto (PostgreSQL)

```elixir
defmodule MyApp.Repo.Migrations.AddIndexToOrdersUser do
  use Ecto.Migration

  # CONCURRENTLY cannot run inside a transaction
  @disable_ddl_transaction true
  @disable_migration_lock true

  def change do
    create_if_not_exists index(:orders, [:user_id, :inserted_at], concurrently: true)
  end
end
```

## Safe Column Addition

```elixir
defmodule MyApp.Repo.Migrations.AddTrackingCodeToOrders do
  use Ecto.Migration

  def change do
    alter table(:orders) do
      # Always nullable in phase 1
      add :tracking_code, :string, null: true
    end
  end
end
```

## Oban Backfill Worker Pattern

```elixir
defmodule MyApp.Workers.BackfillTrackingWorker do
  use Oban.Worker, queue: :background, max_attempts: 3

  import Ecto.Query
  alias MyApp.Repo
  alias MyApp.Orders.Order

  @batch_size 500

  @impl Oban.Worker
  def perform(%Oban.Job{args: %{"cursor_id" => cursor_id}}) do
    orders =
      Order
      |> where([o], o.id > ^cursor_id and is_nil(o.tracking_code))
      |> order_by([o], asc: o.id)
      |> limit(@batch_size)
      |> Repo.all()

    case orders do
      [] ->
        :ok

      batch ->
        # Update batch
        Enum.each(batch, fn order ->
          order
          |> Ecto.Changeset.change(tracking_code: "TRK-#{order.id}")
          |> Repo.update!()
        end)

        last_id = List.last(batch).id
        # Enqueue next slice
        %{"cursor_id" => last_id}
        |> MyApp.Workers.BackfillTrackingWorker.new()
        |> Oban.insert()

        :ok
    end
  end
end
```
