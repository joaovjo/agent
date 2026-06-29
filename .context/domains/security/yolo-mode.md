# YOLO Mode

## Visão Geral
Flag `--yolo` que desativa temporariamente as proteções de segurança para operações de alto risco. Projetado para uso consciente em ambientes controlados (CI, containers, projetos pessoais).

## Comportamento
### Ativado (`--yolo`)
- Auto-aprova permissões Critical (rm -rf, sudo, curl | sh)
- Permite execução de comandos arbitrários
- Desativa filesystem jail
- Log completo de todas as operações

### Desativado (default)
- Deny-by-default em todas as categorias
- Toda operação Critical bloqueada
- Filesystem jail ativo
- Requer aprovação humana para ações destrutivas

## Quando Usar
- **Apropriado**: CI/CD, devcontainers, projetos pessoais, prototipação rápida
- **Inapropriado**: Produção, multi-tenant, ambientes compartilhados, dados sensíveis

## Quando NÃO Usar
- Projetos com dados sensíveis (senhas, chaves, PII)
- Ambientes de produção
- Trabalho em equipe (outros podem executar comandos perigosos)
- Codebase que você não conhece

## Safety Measures
1. YOLO mode expira ao fim da sessão
2. Arquivo `.pandow-yolo` no projeto impede ativação sem confirmação
3. Log visível de que YOLO mode está ativo
4. Não pode ser ativado em modo headless sem confirmação

## Implementação
```rust
struct YoloConfig {
    enabled: bool,
    session_id: Uuid,
    expires_at: SystemTime,
    operations_count: u64,
    warnings_shown: Vec<String>,
}
```

## UX
```
⚠️  YOLO MODE ATIVO
Proteções de segurança desativadas.
Comandos perigosos serão executados sem confirmação.
Use apenas em ambientes controlados.
Digite --no-yolo para desativar.
```

## Anti-Patterns
- YOLO mode como default
- YOLO mode permanente
- Não logar operações em YOLO mode
- Ignorar warnings de segurança