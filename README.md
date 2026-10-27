# Elixir Skills

## Objetivo

Skills para desenvolvimento Elixir com Mix, OTP, Phoenix, LiveView, Ecto, Oban, Ash, Nx, segurança, testes e qualidade de entrega.

Este pacote não é uma aplicação final. Ele é uma coleção portátil de instruções para agentes de código trabalharem em projetos reais com padrões consistentes.

## Compatibilidade

| Agente | Entrada recomendada |
| --- | --- |
| Cursor | `.cursor/rules/skills.mdc` e aliases em `.cursor/skills/<skill-name>` |
| Codex | `skills/<skill-name>/SKILL.md` ou cópia/symlink em `$CODEX_HOME/skills` |
| Claude Code | `CLAUDE.md`, depois `skills/<skill-name>/SKILL.md` |
| GitHub Copilot | `.github/copilot-instructions.md`, apontando para `AGENTS.md` e `skills/` |

## Estrutura

- `AGENTS.md`: instruções operacionais para agentes.
- `CLAUDE.md`: ponto de entrada curto para Claude Code.
- `.cursor/rules/skills.mdc`: regra de descoberta para Cursor.
- `.cursor/skills/<skill-name>`: symlink para `skills/<skill-name>`.
- `.github/copilot-instructions.md`: instruções curtas para GitHub Copilot.
- `skills/<skill-name>/SKILL.md`: fonte canônica da skill.
- `skills/<skill-name>/reference.md`: detalhes carregados somente quando a skill pedir.

## Stack Coberta

- Elixir
- Erlang/OTP
- Mix
- Phoenix
- LiveView
- Ecto
- Oban
- Ash
- Nx

## Como Escolher Skills

1. Leia `AGENTS.md` para as regras gerais da coleção.
2. Escolha a menor skill que cobre a tarefa.
3. Use `elixir-core-mix-otp` como baseline quando a tarefa cruzar vários temas.
4. Leia `reference.md` apenas quando a skill ou a complexidade da tarefa pedir.
5. Valide com os comandos disponíveis no projeto alvo.

## Skills Disponíveis

| Skill | Quando usar |
| --- | --- |
| `elixir-ash-framework` | Ash Framework development: resources, domains, actions, policies, calculations, AshPostgres. Trigger on Ash tasks. |
| `elixir-background-jobs-oban` | Oban background jobs: workers, queues, retries, idempotency, unique jobs. Trigger on async and background jobs. |
| `elixir-code-generation-contract` | Mandatory output contract for Elixir/Phoenix: Context boundary, Changesets, ExUnit tests. Trigger on new feature slices. |
| `elixir-core-mix-otp` | Elixir/OTP core: GenServer, supervisors, Tasks, Registry, runtime config. Trigger on OTP and Mix architecture. |
| `elixir-ecto-data-performance` | Ecto data performance: schemas, queries, transactions, N+1 prevention, indexes. Trigger on Ecto DB access. |
| `elixir-nx-ml` | Numerical Elixir with Nx: tensors, defn, EXLA, Axon, Bumblebee, Scholar. Trigger on ML or numerical pipelines. |
| `elixir-phoenix-api` | Phoenix REST APIs: routers, controllers, JSON views, auth, pagination, ConnCase. Trigger on Phoenix API endpoints. |
| `elixir-phoenix-liveview` | Phoenix LiveView: HEEx components, forms, streams, assigns, PubSub, LiveViewTest. Trigger on LiveView UI. |
| `elixir-security-auth` | Elixir/Phoenix security: sessions, tokens, policies, tenant isolation, PII, rate limiting. Trigger on auth and security. |
| `elixir-systematic-debugging` | Elixir systematic debugging: minimal ExUnit test reproducers, single-variable fixes. Trigger when diagnosing bugs. |
| `elixir-testing-quality` | Elixir testing quality: ExUnit, DataCase, ConnCase, Mox, Credo, Dialyzer. Trigger when writing or reviewing tests. |
| `elixir-zero-downtime-migrations` | Zero-downtime migrations in Ecto: concurrent indexes, Expand/Contract, backfills. Trigger on Ecto migrations. |

## Qualidade e Validação

Execute o menor conjunto relevante que existir no projeto alvo:

- `mix test`
- `mix format --check-formatted`
- `mix credo`
- `mix dialyzer`
- `mix compile --warnings-as-errors`

## Notas de Uso

- Confirme dependências reais em mix.exs antes de aplicar skills opcionais.
- Use elixir-security-auth quando houver identidade, tenant, uploads, PII ou segredo.
- Use elixir-testing-quality ao implementar, revisar ou finalizar mudanças.

## Instalação por Symlink

Para Codex:

```bash
mkdir -p "$HOME/.codex/skills"
for d in skills/*; do
  name="$(basename "$d")"
  ln -sfn "$(pwd)/$d" "$HOME/.codex/skills/$name"
done
```

Para Claude Code:

```bash
mkdir -p "$HOME/.claude/skills"
for d in skills/*; do
  name="$(basename "$d")"
  ln -sfn "$(pwd)/$d" "$HOME/.claude/skills/$name"
done
```

Para Cursor em um projeto consumidor:

```bash
mkdir -p .cursor/skills
for d in skills/*; do
  name="$(basename "$d")"
  ln -sfn "../../skills/$name" ".cursor/skills/$name"
done
```

## Prompt Base

```text
Siga AGENTS.md, escolha a menor skill aplicável em skills/<nome>/SKILL.md e carregue reference.md somente se necessário.

Contexto:
- Stack: Elixir, Erlang/OTP, Mix, Phoenix, LiveView, Ecto, Oban, Ash, Nx
- Objetivo: <descreva a tarefa>
- Restrições: <auth, dados, UX, performance, compatibilidade>
- Validação esperada: <testes/build/lint>
```
