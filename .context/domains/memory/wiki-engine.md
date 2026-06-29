# Wiki Engine (LWM + TOON)

## Processo de Ingestão
1. **Receber fonte** (arquivo, URL, texto)
2. **Extrair metadados** (título, autor, data, fonte)
3. **Dividir em chunks** semânticos (parágrafos, seções)
4. **Gerar embedding** para cada chunk (usando modelo de embedding)
5. **Arquivar fonte original** em `.context/raw/` com hash SHA-256
6. **Processar com LLM** para extrair:
   - Entidades mencionadas (pessoas, projetos, conceitos)
   - Decisões/chaves de aprendizado
   - Relações entre entidades
7. **Atualizar wiki**:
   - Criar/atualizar páginas de entidades
   - Atualizar páginas conceituais
   - Adicionar/atualizar referências cruzadas
   - Detectar e registrar contradições
8. **Atualizar índices**:
   - `index.md` (lista de páginas com resumos)
   - `log.md` (entrada cronológica)
   - Banco vetorial (embeddings + metadados)

## Processo de Query
1. **Receber consulta** em linguagem natural
2. **Entender intenção** (via LLM)
3. **Busca híbrida**:
   - BM25 sobre texto completo (texto bruto)
   - Busca vetorial sobre embeddings
   - Fusão de resultados com re-ranking
4. **Recuperar contexto relevante** (top-k chunks)
5. **Gerar resposta** usando LLM com contexto
6. **Atribuir fontes** (citando arquivos em .context/raw/)
7. **Armazenar interação** no log (permitindo learn-back)

## Processo de Lint (Validação de Integridade)
Executa periodicamente para manter qualidade da wiki:

### Verificações Automáticas
- **Páginas órfãs**: páginas sem links de entrada
- **Links quebrados**: referências para páginas inexistentes
- **Contradições detectadas**: via análise de conflitos de fatos
- **Páginas muito grandes**: sugerir divisão
- **Páginas muito pequenas**: sugerir consolidação
- **Conteúdo desatualizado**: baseado em data da fonte mais recente

### Saída do Lint
- Relatório markdown em `.context/lint-report.md`
- Issues para correção (opcionalmente criar PRs automáticas)
- Métricas de saúde da wiki:
  - Cobertura de tópicos
  - Densidade de links
  - Taxa de contradições
  - Freshness média

## Estrutura de Pastas
```
.context/
├── raw/                    # Fontes originais (hash-based naming)
│   └── a1b2c3d4...        # Conteúdo original + metadata.json
├── wiki/                   # Conteúdo gerado pelo LLM (markdown)
│   ├── entities/           # Pessoas, projetos, conceitos específicos
│   │   ├── alice.md        # Pessoa específica
│   │   └── project-x.md    # Projeto específico
│   ├── concepts/           # Conceitos abstratos, metodologias
│   │   ├── tdd.md
│   │   └── clean-architecture.md
│   ├── index.md            # Lista de todas as páginas + resumos
│   └── log.md              # Registro cronológico de ingestões
├── schema/                 # Definições de tipos e constraints
│   └── entity-schema.json  # Validação JSON Schema
└� vector-store/             # Índices para busca
    ├── embeddings.idx      # FAISS/Annoy/HNSW index
    └── metadata.json       # Mapeamento ID -> fonte/chunk
```

## Formato de Página Wiki
```markdown
# Título da Página

## Metadados
- **Tipo**: entity|concept|process
- **Fonte Primária**: hash ou referência
- **Atualizado**: YYYY-MM-DD
- **Versão**: 1.0

## Resumo
[Descrição concisa]

## Detalhes
[Seções conforme necessário]

## Referências
- [Fonte 1](#)
- [Fonte 2](#)

## Relacionado
- [[Página Relacionada 1]]
- [[Página Relacionada 2]]

## Histórico
- 2026-06-28: Criada a partir de fonte X
- 2026-06-29: Atualizada com fonte Y
```

## Detecção de Contradições
Durante ingestão, comparar novo conteúdo com fatos existentes:

1. **Extrair afirmações** do novo conteúdo (via LLM)
2. **Buscar afirmações similares** no wiki existente
3. **Comparar semântica** (não apenas texto literal)
4. **Classificar relação**:
   - **Confirmada**: Novo apoia existente
   - **Estendida**: Novo adiciona nuances
   - **Contradiz**: Novo nega ou modifica significativamente
   - **Não relacionada**: Tópicos diferentes
5. **Ações por tipo**:
   - Confirmada/Estendida: Atualizar normalmente
   - Contradiz: Criar página de tensão/contradição (estilo Antinomia)
   - Não relacionada: Processar normalmente

## Implementação Técnica
- **Biblioteca TOON**: Usar implementação Rust do toon-format/toon
- **Embeddings**: Sentence-Transformers (paraphrase-multilingual-MiniLM-L12-v2)
- **Banco Vetorial**: Inicial com faixa linear, migrar para HNSW/FAISS em escala
- **Cache LRU**: Para consultas frequentes
- **Atualização em Lote**: Processar múltiplas fontes de uma vez
- **Webhooks**: Para notificar quando novo conteúdo está disponível

## Métricas de Qualidade
Tracked in `.context/metrics.json`:
- `total_pages`: Contagem de páginas wiki
- `avg_links_per_page`: Densidade de conexão
- `contradiction_rate`: % de páginas com contradições registradas
- `freshness_days`: Idade média das fontes
- `entity_coverage`: % de entidades mencionadas que têm página
- `update_frequency`: Ingestões por dia/semana