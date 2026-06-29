# Mobile Client

## Visão Geral
Aplicação mobile usando Tauri mobile (WebView nativo + Rust backend). Foco em consulta rápida, revisão de código e notificações — não em edição pesada.

## Stack
- **Framework**: Tauri Mobile (WebView + Rust)
- **Frontend**: SvelteKit (adaptado para mobile)
- **Backend**: Rust (Tauri commands, compartilhado com desktop)
- **Comunicação**: WebSocket para pandow-server

## Features
- Chat com streaming de LLM
- Code viewer com syntax highlighting (read-only)
- Notificações push (build completo, PR review)
- Quick actions (comandos frequentes)
- Compartilhamento de sessão com desktop
- Autenticação biométrica

## Limitações
- Read-only para edição de arquivos
- Tela pequena limita visualização
- Bateria/performance limitados
- Não substitui clientes desktop

## Casos de Uso
- Revisão rápida de PR
- Aprovação de permissões
- Consulta de status de CI/CD
- Notificações de eventos importantes
- Comandos rápidos (deploy, rollback)

## UX
- Bottom sheet para navegação
- Gestos (swipe para aprovar/negar)
- Modo escuro nativo
- Haptic feedback para notificações
- Voice input (opcional)

## Anti-Patterns
- Tentar replicar todas as funções do desktop
- Interface complexa em tela pequena
- Ignorar economia de bateria
- Funcionalidades que exigem teclado físico