# Web Client

## Visão Geral
Interface web para o Pandow Code. Comunica-se com um servidor remoto via WebSocket. Útil para acesso remoto a workspaces e uso em equipe.

## Stack
- **Frontend**: SvelteKit (SSR + SPA)
- **Backend API**: Rust (actix-web/axum via pandow-server)
- **Comunicação**: WebSocket + REST
- **Auth**: JWT + OAuth2 (GitHub, GitLab)
- **Deploy**: Docker, Vercel, Railway

## Features
- Chat com streaming de LLM
- File explorer web
- Code viewer com syntax highlighting
- Terminal web embutido (xterm.js)
- Compartilhamento de sessão
- Histórico de sessões
- Multi-workspace

## Rotas
```
/                     → Dashboard
/w/<workspace>        → Workspace chat
/w/<workspace>/files  → File explorer
/w/<workspace>/git    → Git interface
/settings             → User settings
/sessions             → Session history
```

## Limitações
- Requer servidor remoto rodando
- Latência maior que clientes nativos
- Depende de conexão de rede
- Terminal web limitado vs terminal real

## Segurança
- Autenticação obrigatória
- Sessões com expiração
- CORS configurado
- Rate limiting
- Auditoria de operações

## Anti-Patterns
- Web client como único cliente (deve ser complementar)
- Interface pesada que poderia ser TUI/CLI
- Falta de fallback offline