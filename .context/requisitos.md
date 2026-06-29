# Requisitos

## Funcionais

- [ ] **Loop agente**: Recebe prompt em linguagem natural, planeja passos, executa ferramentas, observa resultados, itera até conclusão
- [ ] **Terminal PTY**: Executa comandos em pseudo-terminal real (suporta interativos como vim, python, shell)
- [ ] **Suporte multiplataforma**: Bash (Linux/macOS) e PowerShell 7+ (Windows)
- [ ] **Ferramentas built-in**:
  - Sistema de arquivos: leitura, escrita, edição, busca (glob, grep)
  - Terminal: execução de comandos com PTY
  - Web: busca, fetch, raspagem de conteúdo
  - Git: operações básicas (status, add, commit, log, diff)
  - LSP: navegação de símbolos, definições, referências
  - Task: execução e gerenciamento de tarefas assíncronas
- [ ] **Multi-client**: 
  - CLI: interface de linha de comando rica
  - TUI: interface textual com painéis (chat, arquivos, logs, diff)
  - Desktop: aplicativo nativo (Tauri recomendado)
  - Web: aplicativo WASM/WebSocket
  - Mobile: aplicativo nativo (Tauri mobile)
- [ ] **Proxy LLM unificado**:
  - Suporte a Anthropic (Claude) e OpenAI (GPT)
  - Extensível para modelos locais (Ollama, llama.cpp)
  - Streaming de tokens em tempo real
  - Funções/tools calling
  - Embeddings para busca semântica
- [ ] **Sistema de memória persistente**:
  - Formato TOON (Token-Oriented Object Notation) para eficiência
  - Estrutura .context/ no diretório raiz do projeto alvo
  - Wiki engine: ingest, query, lint (estilo LLM Wiki)
  - Índice vetorial para busca semântica no codebase e documentação
  - Log cronológico de sessões (estilo LLM Wiki)
- [ ] **Persistência de sessão**: Capacidade de retomar trabalho após reinicialização
- [ ] **Controle de versão**: Integração com Git para commits automáticos de mudanças

## Não-funcionais

- [ ] **Performance**: Implementado em Rust para baixa latência e alto throughput
- [ ] **Segurança**: 
  - Modelo deny-by-default (nada é permitido sem permissão explícita)
  - Flag --yolo opcional para bypass total (usuário consciente do risco)
  - Matriz de permissões granular por ferramenta e padrão
  - Log de auditoria para todas as ações
  - Detecção de vazamento de secrets pré-execução
  - Isolamento via sandbox (filesystem jail ou containers)
- [ ] **Portabilidade**: Compila e roda em Linux, macOS, Windows
- [ ] **Extensibilidade**: Arquitetura de plugins para ferramentas e skills customizadas
- [ ] **Observabilidade**: Logging estruturado, métricas de performance, tracing
- [ ] **Manutenibilidade**: Código limpo, bem documentado, com testes de integração

## Restrições

- [ ] **Agnosticismo LLM**: Nenhuma vinculação a provider específico de modelo
- [ ] **Execução local**: Deve funcionar em máquina do usuário ou containers (não requer cloud obrigatório)
- [ ] **Open source**: Licença permissiva (MIT/Apache-2.0)
- [ ] **Transparência**: Arquivos .context/ são legíveis e editáveis por humanos
- [ ] **Compatibilidade**: Funciona com projetos existentes sem modificação necessária
- [ ] **Respeito ao workspace**: Nunca modifica arquivos fora do diretório do projeto sem permissão explícita