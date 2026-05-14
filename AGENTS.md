# Elixir Skills Agent Instructions

Use este arquivo como orientação operacional para Cursor, Codex, Claude Code e GitHub Copilot.

## Princípios

- Use a menor skill aplicável em `skills/<skill-name>/SKILL.md`.
- Trate `skills/` como fonte canônica; `.cursor/skills/` contém apenas aliases.
- Leia `reference.md` somente quando a skill pedir ou quando a tarefa exigir detalhe adicional.
- Inspecione as convenções do projeto alvo antes de criar novos padrões.
- Prefira linguagem e passos agnósticos de ferramenta.
- Preserve segurança, validação, testes e performance como critérios explícitos de entrega.

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

## Ordem de Decisão

1. Identifique o domínio real da tarefa e as dependências já presentes no projeto alvo.
2. Escolha a skill principal mais específica.
3. Use `elixir-core-mix-otp` como baseline quando houver múltiplas áreas.
4. Adicione skills complementares só quando houver risco real de segurança, dados, UX, performance ou testes.
5. Antes de editar, leia arquivos vizinhos e padrões existentes.
6. Ao finalizar, reporte validações executadas e pendências.

## Qualidade Mínima

- Confirme dependências reais em mix.exs antes de aplicar skills opcionais.
- Use elixir-security-auth quando houver identidade, tenant, uploads, PII ou segredo.
- Use elixir-testing-quality ao implementar, revisar ou finalizar mudanças.
- Código deve seguir padrões locais do projeto alvo.
- Mudanças devem ser pequenas, verificáveis e fáceis de revisar.
- Não introduza frameworks, bibliotecas ou serviços opcionais só porque uma skill existe.

## Comandos de Validação

Use o menor conjunto que existir no projeto alvo:

- `mix test`
- `mix format --check-formatted`
- `mix credo`
- `mix dialyzer`
- `mix compile --warnings-as-errors`

## Compatibilidade por Agente

- Cursor: use `.cursor/rules/skills.mdc` e `.cursor/skills/<skill-name>`.
- Codex: use `skills/<skill-name>/SKILL.md` ou instale em `$CODEX_HOME/skills`.
- Claude Code: comece por `CLAUDE.md` e depois pela skill específica.
- GitHub Copilot: mantenha `.github/copilot-instructions.md` curto e apontando para este arquivo.

## Formato de Resposta Recomendado

1. Objetivo
2. Arquivos alterados
3. Decisões de implementação
4. Testes e validação
5. Riscos, suposições e pendências
