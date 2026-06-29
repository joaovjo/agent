# Log de Elaboração

Log cronológico das decisões e eventos durante a elaboração do Pandow Code.

---

## [2026-06-29] Sessão 2 - Agentes, Skills e Domínios

### Eventos
- Criação de 8 agentes em `.opencode/agents/`: elaborador, orchestrator, swe-frontend, swe-backend, swe-infra, qa, architect, researcher
- Criação de 4 skills em `.opencode/skills/`: grill-with-docs, tdd, arch-review, prototype
- Preenchimento completo de 4 domínios vazios (11 arquivos):
  - `domains/tools/`: pty-engine, tool-system, sandbox
  - `domains/security/`: permission-model, yolo-mode
  - `domains/clients/`: cli, tui, desktop, web, mobile
  - `domains/llm/`: unified-proxy, providers, streaming
- Atualização de `opencode.json` com references para agents e skills
- Git commit: `b8a1050` — `feat: adicionar agentes, skills e dominios de elaboracao`

### Decisões
- Skills seguem formato SKILL.md com frontmatter name + mode
- Agentes em .md separados com frontmatter description + mode + color
- Domínios seguem hierarquia flat dentro de `domains/<dominio>/`
- opencode.json registra agents e skills como references

### Próximos passos
- Adaptar skills do mattpocock para o ecossistema Rust do Pandow Code
- Iniciar MVP "Sistema Nervoso": prompt → LLM → PTY → output → LLM
- Criar `pandow-core` crate com loop cognitivo básico

---

## [2026-06-29] Sessão Inicial - Estrutura Base

### Eventos
- Consulta concluída de 10 referências externas
- Criação da estrutura `.context/` com 5 arquivos na raiz
- Definição de 7 domínios em subdiretórios

### Decisões
- ADR-001 a ADR-014 aprovadas (ver `decisoes.md`)
- Schema TOON validado para memória persistente
- Arquitetura hierárquica multi-agente definida
- Security model: deny-by-default + --yolo

### Próximos passos
- Workshop de identidade (SOUL) - aguardando input do usuário
- Detalhar `domains/memory/` (TOON schema, wiki engine, vector-graph)
- Detalhar `domains/agents/` (papéis, orchestration, handoffs)

---

## Como Contribuir

1. Adicione novas entradas no topo (data mais recente primeiro)
2. Use formato: `## [YYYY-MM-DD] Título`
3. Liste eventos, decisões, próximos passos
4. Seja conciso mas informativo