# CLI Client

## Visão Geral
Cliente de linha de comando primário do Pandow Code. Interface direta via terminal para interação com o agente.

## Features
- Execução via `pandow <prompt>` ou `pandow "instrução"`
- Modo interativo com readline (rustyline)
- Suporte a pipe: `cat input.txt | pandow`
- Streaming de resposta do LLM em tempo real
- Colorização de output (terminal)
- Progress indicators para operações longas

## Comandos
```
pandow <prompt>              # Executa prompt único
pandow -i / --interactive    # Modo interativo
pandow watch <file>          # Watch mode (reage a mudanças)
pandow init [path]           # Inicializa .pandow no projeto
pandow config [key] [value]  # Visualiza/altera configuração
pandow doctor                # Diagnóstico do ambiente
pandow --yolo                # Ativa modo inseguro
pandow --version             # Versão
pandow --help                # Ajuda
```

## Rust Crate
**Nome**: `pandow-cli`
**Dependências**: `clap`, `rustyline`, `tokio`, `colored`, `indicatif`

## UI/UX
- Prompt: `❯ pandow "refatore este módulo"`
- Streaming: tokens aparecendo em tempo real
- Tool calls: indicador visual [tool_name(args...)]
- Erros: vermelho, com sugestão
- Warning: amarelo
- Info: azul/ciano
- Aprovação de permissão: [y/N] com explicação

## Anti-Patterns
- Output muito verboso
- Falta de feedback visual para operações longas
- Esquecer de tratar CTRL+C
- Não suportar ANSI fallback