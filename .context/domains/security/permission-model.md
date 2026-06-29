# Permission Model

## Visão Geral
Modelo de permissões deny-by-default do Pandow Code. Inspirado em sistemas como Android, SELinux e Okta — mínimo privilégio, escopo explícito, auditoria completa.

## Princípios
1. **Deny-by-Default**: Tudo é negado até explicitamente permitido
2. **Least Privilege**: Ferramentas e agentes têm mínimo necessário
3. **Explicit Scope**: Permissões são granulares e específicas
4. **Auditability**: Toda decisão de permissão é logada
5. **Revocability**: Permissões podem ser revogadas a qualquer momento
6. **Transparency**: Usuário vê o que está sendo solicitado

## Hierarquia de Permissões
```
                    ┌──────────────┐
                    │   Default    │ (deny all)
                    └──────┬───────┘
                           │
              ┌────────────┼────────────┐
              │            │            │
        ┌─────┴─────┐ ┌───┴───┐ ┌─────┴─────┐
        │  Session  │ │Project│ │   User    │
        │ (auto)    │ │ (auto)│ │ (explicit)│
        └───────────┘ └───────┘ └───────────┘
```

## Categorias de Permissão
### File System
- `fs:read:<path>` — Leitura de arquivos
- `fs:write:<path>` — Escrita em arquivos
- `fs:delete:<path>` — Remoção de arquivos
- `fs:execute:<path>` — Execução de binários

### Network
- `net:connect:<host:port>` — Conexão de saída
- `net:listen:<port>` — Servidor local
- `net:dns:<domain>` — Resolução DNS

### System
- `sys:env:read:<var>` — Leitura de variável de ambiente
- `sys:env:set:<var>` — Modificação de env var
- `sys:process:spawn` — Criação de processo
- `sys:process:signal` — Envio de sinal

### Admin
- `admin:install` — Instalação de pacotes
- `admin:config` — Modificação de config do sistema
- `admin:network` — Modificação de config de rede

## Mecanismo de Aprovação
1. Tool solicita permissão ao Permission Manager
2. Manager verifica cache de sessão
3. Se não em cache, cria PermissionRequest e mostra ao usuário
4. Usuário aprova/nega com opção de "lembrar por sessão"
5. Decisão é cacheada e logada

## Formato de Log
```json
{
  "timestamp": "2026-06-29T19:00:00Z",
  "permission": "fs:delete:/project/temp",
  "decision": "approved",
  "reason": "Arquivo temporário de build",
  "risk_level": "Dangerous",
  "session_cached": true,
  "tool": "file_ops"
}
```

## Anti-Patterns
- Wildcards muito amplos (fs:write:**)
- Permissões permanentes sem necessidade
- Aprovação sem contexto
- Cache sem expiração
- Ignorar permissões negadas