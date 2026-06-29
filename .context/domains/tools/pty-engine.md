# PTY Engine

## Visão Geral
O PTY Engine é o coração da execução de comandos do Pandow Code. Ele gerencia processos de terminal (pseudo-terminais) para executar comandos, capturar saída, e interagir com processos longos.

## Arquitetura
```
┌─────────────────────┐
│   Tool Executor     │  ← Interface unificada de execução
├─────────────────────┤
│    PTY Engine       │  ← Gerenciamento de pseudo-terminais
├─────────────────────┤
│ ┌─────────────────┐ │
│ │   PTY Process   │ │  ← Processo real no PTY
│ └─────────────────┘ │
│ ┌─────────────────┐ │
│ │   PTY Process   │ │  ← Múltiplos processos simultâneos
│ └─────────────────┘ │
└─────────────────────┘
```

## Funcionalidades
- Criação e gerenciamento de pseudo-terminais
- Execução de comandos com timeout
- Streaming de saída em tempo real
- Redirecionamento de stdin/stdout/stderr
- Suporte a pipelining de comandos
- Detecção de deadlock/loop infinito
- Controle de processos (kill, pause, resume)

## Rust Crate
**Nome sugerido**: `pandow-pty` ou incorporado em `pandow-tools`
**Dependências sugeridas**: `portable-pty`, `tokio`, `command-group`

## Casos de Uso
1. **Shell interativo**: Comandos únicos (git, cargo, npm)
2. **Processos longos**: Dev servers, compilação, testes
3. **Pipelines**: `cargo build && cargo test`
4. **Shell scripting**: Scripts complexos multi-linha

## Anti-Patterns
- Prompt parsing frágil (use PTY I/O em vez de prompt matching)
- Timeout muito curto para compilações
- Não tratar EAGAIN/EINTR corretamente
- Buffer ilimitado para output