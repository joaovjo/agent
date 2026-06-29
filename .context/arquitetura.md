# Arquitetura

## Visão Geral

```
┌─────────────────────────────────────────────────────────────┐
│                    Pandow Code Architecture                  │
├─────────────────────────────────────────────────────────────┤
│                                                             │
│  ┌─────────┐   ┌─────────┐   ┌─────────┐   ┌─────────┐    │
│  │   CLI   │   │   TUI   │   │ Desktop │   │   Web   │    │
│  │ (clap)  │   │(ratatui)│   │ (Tauri) │   │ (WASM)  │    │
│  └────┬────┘   └────┬────┘   └────┬────┘   └────┬────┘    │
│       │             │             │             │          │
│       └─────────────┴──────┬──────┴─────────────┘          │
│                            │                                │
│                     ┌──────▼──────┐                       │
│                     │   server/   │                       │
│                     │  (gRPC+WS)  │                       │
│                     └──────┬──────┘                       │
│                            │                                │
│         ┌──────────────────┼──────────────────┐           │
│         │                  │                  │           │
│  ┌──────▼──────┐   ┌──────▼──────┐   ┌──────▼──────┐      │
│  │    core/    │   │    llm/     │   │  (tools)    │      │
│  │  (PTY,mem)  │   │(proxy,chat) │   │  (bash,pwsh)│      │
│  └─────────────┘   └─────────────┘   └─────────────┘      │
│                                                             │
└─────────────────────────────────────────────────────────────┘
```

## Stack Tecnológica

| Camada | Tecnologia | Justificativa |
|--------|-----------|---------------|
| Backend | **Rust** | Performance, segurança memory-safe, ecossistema moderno |
| Workspace | **Cargo** | Gerenciamento de dependências, monorepos, testes |
| IPC | **gRPC + WebSocket** | Comunicação bidirecional eficiente entre clients e server |
| Terminal | **PTY (portable-pty/vte)** | Suporte a processos interativos cross-platform |
| LLM Client | **Unified Proxy** | Agnóstico a providers (OpenAI, Anthropic, locais) |
| Memory | **TOON + Vector Index** | Eficiência de tokens + busca semântica |
| Sandbox | **filesystem jail + devcontainers** | Segurança por padrão, opção containers |
| Desktop | **Tauri** | Rust + WebView, leve e seguro |
| Mobile | **Tauri mobile** | Extensão natural do desktop |
| Web | **WASM + WebSocket** | Compartilha backend com desktop |
| Logging | **tracing + acquisição** | Structured logging sem ruído (loggingsucks.com) |

## Crates

```
pandow-code/
├── Cargo.toml           # Workspace definition
├── crates/
│   ├── core/            # PTY engine, file ops, memory (TOON), tools base
│   ├── llm/             # Unified proxy, chat, streaming, embeddings
│   ├── server/          # gRPC + WebSocket daemon
│   └── cli/             # CLI + TUI (ratatui) client
└── .context/            # Memory perto do projeto alvo
```

## Arquitetura Cognitiva

### Loop Principal
```
User Prompt
     ↓
┌─────────────────┐
│ Orchestrator    │ ←→ Agentes especializados (subagentes)
│ (planeja)       │
└────────┬────────┘
         ↓
    ┌────▼────┐
    │  Task   │ ←→ Tools (fs, bash, web, git, task)
    │ Executor│
    └────┬────┘
         ↓
   Observation
         ↓
    ┌────▼────┐
    │  Judge  │ ←→ Critério de sucesso
    └─────────┘
```

### Subagentes (obrigatórios)
| Papel | Responsabilidade |
|-------|-----------------|
| **Orchestrator** | Decompor problema, delegar, validar, repetir |
| **swe-frontend** | UI/UX, componentes, styling, acessibilidade |
| **swe-backend** | APIs, lógica de negócio, infraestrutura |
| **swe-infra** | CI/CD, deploy, observabilidade, segurança |
| **QA** | Testes, bug hunting, verificação, security review |
| **Architect** | Decisões estruturais, ADRs, tradeoffs |
| **Researcher** | Web browsing, docs, best practices |

## Sistema de Memória

### TOON Schema
```
.context/
├── visao-de-produto.md      # Frontmatter: {version, updated}
├── requisitos.md
├── arquitetura.md
├── decisoes.md              # Cada ADR é um bloco TOON
├── referencias.md
├── domains/
│   └── ...
└── log.md                   # Log de eventos (append-only)
```

### Vector Graph
- **Nós**: Arquivos, símbolos, conceitos, decisões
- **Arestas**: dependências, relações, referências
- **Indexação incremental**: só atualiza mudanças
- **Busca híbrida**: BM25 + embeddings (fallback sem vector)

## Segurança

### Permission Matrix (deny-by-default)
```
tools:
  bash:
    "git *": allow
    "rm *": deny
    "chmod *": deny
    "*": ask
  read: allow
  edit: ask
  glob: allow
  grep: allow
```

### --yolo Mode
- Flag opcional que remove todos os controles
- Apenas para usuários experientes
- Log de auditoria marca ações como "yolo-approved"

## Referências
- [microgpt](https://karpathy.github.io/2026/02/12/microgpt/) - Essência algorítmica
- [TOON Format](https://toonformat.dev/) - Formato de memória
- [LLM Wiki](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f) - Padrão de wiki
- [rug-agentic-workflow](https://github.com/github/awesome-copilot/tree/main/plugins/rug-agentic-workflow) - Orquestração simples