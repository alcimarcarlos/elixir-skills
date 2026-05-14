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
| `elixir-ash-framework` | Use for Ash Framework development, including resources, domains, actions, code interfaces, policies, validations, changes, calculations, aggregates, data layers, AshPostgres, AshPhoenix, AshJsonApi, AshGraphql, actor/tenant authorization, testing and architecture decisions. |
| `elixir-background-jobs-oban` | Use for Oban background jobs, workers, queues, retries, scheduling, cron, uniqueness, idempotency, backoff, telemetry, plugins, job arguments, observability, and Oban testing. |
| `elixir-core-mix-otp` | Use for core Elixir application development, Mix projects, OTP architecture, Application callbacks, supervisors, GenServer, Agent, Task, Registry, DynamicSupervisor, PubSub-adjacent process design, runtime config, releases, code review, and architecture decisions. |
| `elixir-ecto-data-performance` | Use for Ecto schemas, changesets, queries, associations, migrations, Repo usage, transactions, Ecto.Multi, preloads, N+1 prevention, indexes, database constraints, data migrations, large datasets, caching-adjacent data access, and performance review. |
| `elixir-nx-ml` | Use for Numerical Elixir work with Nx, tensors, defn, EXLA, Torchx, Axon, Bumblebee, Scholar, Explorer-adjacent data preparation, embeddings, model inference, numerical pipelines, performance review and tests. |
| `elixir-phoenix-api` | Use for Phoenix API design and implementation, including routers, controllers, plugs, JSON responses, REST contracts, API versioning, pagination, error handling, authentication boundaries, OpenAPI-style contracts, async 202 workflows, and ConnCase tests. |
| `elixir-phoenix-liveview` | Use for Phoenix LiveView development, HEEx templates, function components, LiveComponents, forms, streams, assigns, PubSub updates, uploads, live navigation, accessibility, realtime UX, and LiveView tests. |
| `elixir-security-auth` | Use for Elixir/Phoenix security, authentication, authorization, sessions, tokens, Phoenix.Token, plugs, policies, multi-tenant boundaries, uploads, CSRF, CORS, secrets, PII, logging redaction, LiveView security, API security and OWASP-style review. |
| `elixir-testing-quality` | Use for Elixir test writing and quality review, including ExUnit, ConnCase, DataCase, LiveViewTest, Mox, Bypass, StreamData, factories/fixtures, async tests, formatter, Credo, Dialyxir, compile warnings, CI readiness and code review. |

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
