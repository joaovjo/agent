# Desktop Client

## Visão Geral
Aplicação desktop nativa usando Tauri. Interface gráfica completa com todas as funcionalidades do Pandow Code.

## Stack
- **Framework**: Tauri v2
- **Frontend**: SvelteKit (leve, reativo)
- **Backend**: Rust (Tauri commands)
- **Comunicação**: IPC (Tauri invoke) + WebSocket para server remoto

## Features
- Janela principal com chat + workspace explorer
- Dark/light theme
- Split view (código + conversa)
- File explorer integrado
- Git graph visual
- Settings panel
- Plugin manager UI
- Multi-session tabs

## Layout
```
┌──────────────┬──────────────────────────────┐
│  Sidebar     │  Main Area                   │
│  ──────────  │  ──────────                  │
│  Explorer    │  [Conversation / Code]        │
│  Search      │                              │
│  Git         │                              │
│  Extensions  │                              │
│  Settings    │                              │
└──────────────┴──────────────────────────────┘
```

## Comunicação
- Local: IPC via Tauri commands
- Remoto: WebSocket para pandow-server
- Streaming: Server-Sent Events para tokens LLM

## Build
- `cargo tauri build` — produz instaladores nativos
- Suporte: Windows (MSI), macOS (DMG), Linux (AppImage/Flatpak)

## Anti-Patterns
- Over-engineering de UI antes do CLI/TUI estarem maduros
- Dependências pesadas de frontend
- Ignorar acessibilidade
- Performance lenta em projetos grandes