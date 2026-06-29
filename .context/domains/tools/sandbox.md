# Sandbox

## Visão Geral
Sistema de sandbox que isola a execução de comandos e operações do agente, garantindo que ações destrutivas ou perigosas sejam contidas.

## Modos de Sandbox

### 1. Filesystem Jail
- Restringe operações ao workspace do projeto
- Diretórios permitidos configuráveis (allowlist)
- Bloqueia acesso a `/etc`, `~/.ssh`, etc.
- Implementado via change root (chroot) ou bind mounts

### 2. Devcontainers
- Execução dentro de container Docker
- Imagem personalizada com ferramentas pré-instaladas
- Volumes montados somente-leitura para partes do sistema
- Network isolation opcional

### 3. Permission System (Deny-by-Default)
- Toda operação requer permissão explícita
- Categorias: read, write, execute, network, admin
- Ações destrutivas sempre requerem aprovação humana
- Cache de permissões por sessão

## Modelo de Permissões
```rust
enum Permission {
    Read(PathBuf),
    Write(PathBuf),
    Execute(Command),
    Network(NetworkTarget),
    Admin(AdminAction),
}

struct PermissionRequest {
    permission: Permission,
    reason: String,
    context: String,
    risk_level: RiskLevel,
}
```

## Níveis de Risco
| Nível | Exemplos | Ação |
|-------|----------|------|
| Info | read, glob, grep | Auto-aprovado |
| Warning | write, install, run tests | Confirmar com usuário |
| Dangerous | delete, format, chmod | Bloquear, requer override |
| Critical | rm -rf, sudo, curl/sh | Bloqueado sem --yolo |

## Flag --yolo
- Desativa verificações de segurança para Critical
- Log de todas as operações realizadas
- Requer confirmação explícita do usuário
- Expira após sessão

## Implementação
- **Filesystem jail**: Rust `std::fs` wrappers com path canonicalization
- **Devcontainers**: Docker/Devcontainer API
- **Permissions**: Sistema de regras + cache em memória

## Anti-Patterns
- Permitir path traversal (../../etc/passwd)
- Symlink attacks fora do jail
- Esquecer de validar permissões antes de executar
- Cache de permissões sem expiração