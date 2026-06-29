# Referências

Este documento contém resumos de todas as fontes consultadas durante a elaboração do Pandow Code.

## Principais Referências (Já Consultadas)

### SOUL.md
**URL**: https://soul.md/  
**Tipo**: Conceito filosófico  
**Resumo**: Documento sobre identidade de AI — quem o agente é, não só o que faz. A alma é o conjunto de valores, limites e relação com humanos que persiste entre sessões. Para AIs, a alma é externalizada via documentos porque a memória é efêmera entre sessões.  
**Usado em**: `domains/identity/soul-do-agente.md`

### Claude 4.5 Opus Soul Document
**URL**: https://gist.github.com/Richard-Weiss/efe157692991535403bd7e7fb20b6695  
**Tipo**: Constituição de AI  
**Resumo**: Documento completo definindo a identidade do Claude da Anthropic. Inclui: valores hierárquicos (segurança > ética > diretrizes > utilidade), hierarquia operador/usuário, comportamentos hardcoded vs softcoded, agentic behaviors, honesty, harm avoidance. Serve como inspiração direta para o Pandow Code.  
**Usado em**: `domains/identity/soul-do-agente.md`, ADR-007

### microgpt (Karpathy)
**URL**: https://karpathy.github.io/2026/02/12/microgpt/  
**Tipo**: Arquitetura mínima  
**Resumo**: Implementação GPT em 200 linhas Python puro. Componentes: tokenizer (character-level), autograd (Value class), transformer (attention + MLP), Adam optimizer, training/inference loops. Mostro que o algoritmo essencial é pequeno — o resto é eficiência. Link direto para entender o "cerebro" de um agente.  
**Usado em**: `arquitetura.md` (loop cognitivo), Fase 3 MVP

### LLM Wiki (Karpathy)
**URL**: https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f  
**Tipo**: Padrão de memória  
**Resumo**: Três camadas: Raw sources (imutável), Wiki (LLM-mantido, markdown), Schema (CLAUDE.md/AGENTS.md). Operações: Ingest (processa fontes), Query (busca no wiki), Lint (verifica contradições/orphan pages). Wiki como artefato composto — o LLM faz todo o bookkeeping.  
**Usado em**: `domains/memory/`, `decisoes.md` (ADR-005)

### mattpocock/skills
**URL**: https://github.com/mattpocock/skills  
**Tipo**: Skills de engenharia  
**Resumo**: Skills para desenvolvimento com IA: grill (entrevista estruturada), tdd (red-green-refactor), improve-codebase-architecture, domain-modeling, diagnosing-bugs, codebase-design, prototype, grill-with-docs (alinhamento + memory). Foco em skills pequenas, adaptáveis, compostáveis.  
**Usado em**: Skills de engenharia a serem adaptadas

### ai-team-orchestration
**URL**: https://github.com/github/awesome-copilot/tree/main/plugins/ai-team-orchestration  
**Tipo**: Multi-agente workflow  
**Resumo**: Orquestração com papéis: Producer (Remy) — planejamento/sprints/merge, Dev Team (Nova/Sage/Milo) — frontend/backend/visual, QA (Ivy) — testes/bug filing/sign-off. Workflow de sprints, brainstorms, handoffs estruturados.  
**Usado em**: `domains/agents/agent-roles.md`

### rug-agentic-workflow
**URL**: https://github.com/github/awesome-copilot/tree/main/plugins/rug-agentic-workflow  
**Tipo**: Multi-agente simplificado  
**Resumo**: Workflow de 3 agentes: orchestrator (planeja e delega), swe-subagent (implementação), qa-subagent (testes/verificação). Mais simples que ai-team-orchestration, adequado para projetos menores.  
**Usado em**: `domains/agents/orchestration.md`

### TOON Format
**URL**: https://toonformat.dev/  
**Tipo**: Formato de dados  
**Resumo**: Formato compacto para LLMs. ~40% menos tokens que JSON, 76.4% accuracy de parsing vs 75% do JSON. Arrays tabulares, headers de campos explícitos, sintaxe minimalista (como YAML mas com CSV). Implementações em Rust, TypeScript, Python, Go, .NET. Ideal para .context/.  
**Usado em**: `decisoes.md` (ADR-010), `domains/memory/schema-toon.md`

### PandowLabs GitHub
**URL**: https://github.com/pandowlabs  
**Tipo**: Organização/Repositórios  
**Resumo**: Organização existe com 3 repositórios: ninorm (TS), core (TS), .github. Nenhum crate Rust ainda. Base para migração do frontend. Estrutura .github sugere preparação para CI/CD.  
**Usado em**: Planejamento de migração futura

### containers.dev
**URL**: https://containers.dev/  
**Tipo**: Especificação Dev Containers  
**Resumo**: Padrão aberto para desenvolvimento em containers. Define como configurar ambientes de desenvolvimento reproduzíveis. Será usado como base para sandbox containerizado do Pandow Code.  
**Usado em**: `decisoes.md` (ADR-009), sandbox em containers

### loggingsucks.com
**URL**: https://loggingsucks.com/  
**Tipo**: Princípios de logging  
**Resumo**: Visão sobre logging estruturado sem ruído desnecessário. Foco em logs que sejam realmente úteis para debugging e ação imediata. Influenciará design do sistema de logging.  
**Usado em**: `decisoes.md` (ADR-011), logging estruturado

---

## Como Usar Este Documento

- Consulte antes de decisões arquiteturais
- Atualize quando nova referência for consultada
- Mantenha formato: URL, Tipo, Resumo, Usado em

---

*Última atualização: 2026-06-29*