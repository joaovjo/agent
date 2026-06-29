# Schema TOON para .context/

## Over
TOON (Token-Oriented Object Notation) é usado como formato de serialização para todos os arquivos .md do .context/ que contêm estruturas de dados.

## Vantagens de TOON para .context/
- ~40% menos tokens que JSON (economia no contexto do LLM)
- 76.4% accuracy de parsing vs 75% do JSON
- Sintaxe YAML-like (legível por humanos)
- Arrays tabulares para listas grandes
- Headers de campos explícitos

## Estrutura de Arquivos TOON

### Convenções
```
# Nome do Arquivo

## Frontmatter (TOON headers)
version: "1.0"
updated: "2026-06-29"
schema: "pandow-context-v1"

[conteúdo TOON]
```

### Exemplo: Decisões
```toon
decisoes:
  - id: "ADR-001"
    status: "accepted"
    data: "2026-06-29"
    titulo: "Linguagem e Workspace"
    contexto: "..."
    decisao: "..."
    consequencias:
      positivas:
        - "Performance C-like"
        - "Memory safe"
      negativas:
        - "Curva de aprendizado"
    alternativas:
      - "Go"
      - "C++"
```

### Exemplo: Tarefas
```toon
tarefas:
  - id: "T-001"
    prioridade: "alta"
    status: "pendente"
    titulo: "Implementar PTY engine"
    descricao: "Motor de pseudo-terminal para executar comandos interativos"
    labels: ["core", "pty"]
    dependencias: []
    estimativa: "3d"
```

## Arrays Tabulares (Para Listas Grandes)

Seção `requisitos`:

```toon
requisitos:
  N: ID, Tipo, Descrição, Prioridade, Status
  1: RF-001, Funcional, "Loop agente completo", alta, implementado
  2: RF-002, Funcional, "PTY terminal real", alta, pendente
```

## Schema TOON Field Types

| Tipo TOON | Rust Equivalente | Validação |\n|---|---|---|\n| `string` | String | `""` não vazio se required |\n| `number` | f64/i64 | range checks |\n| `array` | Vec<T> | length headers |\n| `object` | structs | schema validation |\n| `boolean` | bool | true/false |\n| `null` | Option | explicit omission |\n\n## Parsing Strategy

1. LLM gera conteúdo TOON no tool output
2. Parser Rust valida schema
3. Em caso de erro, LLM é re-promptado com mensagem de erro específica
4. Arquivo .md é escrito com TOON formatado\n\n## Referências
- TOON Format Spec: https://toonformat.dev/guide/format-overview.html
- Implementação Rust: https://github.com/toon-format/toon