# TUI Client

## Visão Geral
Interface de terminal com layout rico, inspirada em ferramentas como Lazygit, Lazydocker e K9s. Oferece visão multi-painel do estado do agente.

## Layout
```
┌──────────────────────────────────────────────┐
│  Pandow Code v0.1.0            [⌛ waiting]  │  ← Status bar
├──────────────┬───────────────────────────────┤
│  Conversation│  Workspace                    │
│  ─────────── │  ─────────                    │
│  user: ...   │  src/                         │
│  agent: ...  │  ├── main.rs                  │
│  tool: ...   │  ├── lib.rs                   │
│              │  └── tests/                   │
│              │                               │
│              │  [git: main ↑1 ↓0]           │
├──────────────┴───────────────────────────────┤
│  Input: o que você quer fazer?               │  ← Command bar
└──────────────────────────────────────────────┘
```

## Painéis
1. **Conversation** — Histórico da conversa atual
2. **Workspace** — Árvore de arquivos + git status
3. **Tool Output** — Saída de ferramentas em execução
4. **Agent Status** — Estado do agente (thinking, executing, waiting)
5. **Memory** — Contexto carregado do .context/

## Features
- Navegação por teclado (vim-like)
- Splits redimensionáveis
- Modo dividido (ver código + conversa)
- Syntax highlighting
- Múltiplas abas/sessões

## Rust Crate
**Nome**: `pandow-tui`
**Dependências**: `ratatui`, `crossterm`, `tokio`

## Anti-Patterns
- Over-engineering de layout
- Muitos painéis (informação excessiva)
- Falta de atalhos de teclado intuitivos
- Performance lenta em arquivos grandes