# Índice e Log (Estilo LLM Wiki)

## index.md
Índice navegável de todo o conteúdo do `.context/wiki/`.

### Estrutura
```markdown
# Índice do Conhecimento

## Entidades
| Página | Tipo | Resumo | Última Atualização | Links |
|--------|------|--------|-------------------|-------|
| [[Alice]] | Pessoa | Engenheira sênior na equipe X | 2026-06-28 | 12 |
| [[Project-X]] | Projeto | Sistema de automação de CI/CD | 2026-06-29 | 8 |

## Conceitos
| Página | Tipo | Resumo | Última Atualização | Links |
|--------|------|--------|-------------------|-------|
| [[TDD]] | Metodologia | Test-driven development cycle | 2026-06-25 | 15 |
| [[Clean Architecture]] | Arquitetura | Separação de concerns | 2026-06-20 | 10 |

## Fontes Recentes
| Hash | Título | Tipo | Data | Status |
|------|--------|------|------|--------|
| a1b2c3d4... | "TDD Best Practices" | Artigo | 2026-06-28 | Ingerido |

## Estatísticas
- Total de páginas: 47
- Total de links: 128
- Contradições ativas: 2
- Freshness média: 3.2 dias
```

### Atualização
- Incremental durante cada ingestão
- Rebuild completo opcional (lint phase)
- Cache de resumos para performance

---

## log.md
Log cronológico append-only de todas as operações no sistema de memória.

### Formato de Entrada
```markdown
## [2026-06-29 14:32:11] ingest | "TDD Best Practices"
- **Hash**: a1b2c3d4e5f6...
- **Fonte**: https://example.com/tdd-best-practices
- **Páginas afetadas**: [[TDD]], [[Clean Code]], [[Testing]]
- **Entidades novas**: 3 (Kent Beck, JUnit, red-green-refactor)
- **Tempo de processamento**: 2.3s

## [2026-06-29 14:35:42] query | "Como fazer TDD em Rust?"
- **Chunks recuperados**: 5
- **Fontes citadas**: a1b2c3d4..., f0e1d2c3...
- **Tempo de resposta**: 1.1s

## [2026-06-29 15:01:00] lint | Verificação semanal
- **Órfãs detectadas**: 2 ([[OldConcept]], [[DeprecatedTool]])
- **Links quebrados**: 0
- **Contradições**: 1 (TDD vs BDD overlap)
- **Recomendações**: Merge [[TDD]] e [[BDD]] com seção de diferenças
```

### Consultas Úteis
```bash
# Últimas 5 ingestões
grep "^## \[.*\] ingest" log.md | tail -5

# Todas as queries sobre "TDD"
grep -i "query.*tdd" log.md

# Páginas com mais atualizações
grep "Páginas afetadas" log.md | ... | sort | uniq -c | sort -rn
```

---

## Convenções de Wikilinks
- `[[Nome da Página]]` — link interno para página no wiki
- `[[Nome da Página|Texto Exibido]]` — link com texto customizado
- `#hash` — link para seção específica
- Não usar `[[Arquivo.md]]` — apenas o nome lógico da página