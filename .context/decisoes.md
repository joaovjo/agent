# Registro de Decisões de Arquitetura (ADRs)

## ADR-001: Linguagem e Workspace
**Status**: Aceito  
**Data**: 2026-06-29  
**Contexto**: Necessitamos de uma linguagem performática, memory-safe com bom ecossistema para sistemas de baixo nível.  
**Decisão**: Usar Rust com Cargo Workspace monorepo.  
**Consequências**:  
- Positivo: Performance C-like, segurança memory-safe, excelente gerenciamento de dependências  
- Negativo: Curva de aprendizado mais alta que GC languages  
**Alternativas consideradas**: Go (mais simples mas GC), C++ (performance mas complexidade),  

## ADR-002: Arquitetura Cognitiva
**Status**: Aceito  
**Data**: 2026-06-29  
**Contexto**: Precisamos de uma arquitetura que permita raciocínio complexo e uso de ferramentas.  
**Decisão**: Arquitetura hierárquica com subagentes especializados (Orchestrator + Workers).  
**Consequências**:  
- Positivo: Separação de preocupações, especialização, reutilização de workers  
- Negativo: Overhead de comunicação entre agentes  
**Alternativas consideradas**: Single agent com skills, Blackboard architecture  

## ADR-003: Mecanismo de Comunicação
**Status**: Aceito  
**Data**: 2026-06-29  
**Contexto**: Precisamos de comunicação eficiente entre cliente e servidor.  
**Decisão**: gRPC para comandos/queries + WebSocket para streaming em tempo real.  
**Consequências**:  
- Positivo: gRPC eficiente para RPC, WebSocket ideal para streaming de tokens  
- Negativo: Complexidade adicional de dois protocolos  
**Alternativas consideradas**: Apenas gRPC com server-streaming, Apenas WebSocket, REST + WebSocket  

## ADR-004: Engine de Terminal
**Status**: Aceito  
**Data**: 2026-06-29  
**Contexto**: Precisamos executar comandos interativos como shells, editores, REPLs.  
**Decisão**: PTY (Pseudo-terminal) real usando bibliotecas portables.  
**Consequências**:  
- Positivo: Suporte total a processos interativos, comportamento nativo  
- Negativo: Mais complexo que pipes simples, requer tratamento de tamanho de terminal  
**Alternativas consideradas**: Pipes simples, terminal emulado (como xterm.js)  

## ADR-005: Sistema de Memória Persistente
**Status**: Aceito  
**Data**: 2026-06-29  
**Contexto**: Necessitamos de memória que persista entre sessões e seja legível/editável por humanos.  
**Decisão**: Diretório .context/ no projeto alvo com formato TOON + índice vetorial.  
**Consequências**:  
- Positivo: Total transparência, editável manualmente, eficiente em tokens, busca semântica  
- Negativo: Requer sincronização com sistema de arquivos, overhead de indexação  
**Alternativas consideradas**: SQLite, Redis, armazenamento apenas no servidor, apenas arquivos Markdown  

## ADR-006: Proxy LLM Unificado
**Status**: Aceito  
**Data**: 2026-06-29  
**Contexto**: Queremos evitar vendor lock-in e suportar múltiplos providers.  
**Decisão**: Camada de abstração que normaliza APIs de diferentes providers.  
**Consequências**:  
- Positivo: Troca fácil entre providers, suporte a modelos locais, fallback automático  
- Negativo: Abstração pode perder features específicas de providers  
**Alternativas consideradas**: Binding direto a um provider, múltiplos clientes separados  

## ADR-007: Modelo de Segurança
**Status**: Aceito  
**Data**: 2026-06-29  
**Contexto**: Precisamos de segurança robusta para uso empresarial.  
**Decisão**: Deny-by-default com flag --yolo opcional para bypass total.  
**Consequências**:  
- Positivo: Seguro por padrão, controlável pelo usuário, auditável  
- Negativo: Pode ser frustrante inicialmente até aprender as permissões  
**Alternativas consideradas**: Allow-by-default, permissão baseada em roles, nenhum controle  

## ADR-008: Estratégia de Clientes
**Status**: Aceito  
**Data**: 2026-06-29  
**Contexto**: Queremos atingir usuários em diferentes contextos de uso.  
**Decisão**: Suportar CLI, TUI, Desktop, Web e Mobile com backend compartilhado.  
**Consequências**:  
- Positivo: Alcance máximo, experiência consistente, reutilização de lógica de negócio  
- Negativo: Mais trabalho de desenvolvimento e manutenção  
**Alternativas consideradas**: Apenas CLI, Apenas Web, Cliente único com múltiplas views  

## ADR-009: Estratégia de Execução
**Status**: Aceito  
**Data**: 2026-06-29  
**Contexto**: Precisamos equilibrar performance, segurança e conveniência.  
**Decisão**: Suporte tanto para execução nativa no host quanto para containers (devcontainers).  
**Consequências**:  
- Positivo: Flexibilidade para diferentes ambientes de segurança, desempenho nativo quando possível  
- Negativo: Complexidade adicional de suportar dois modos  
**Alternativas consideradas**: Apenas nativo, Apenas containers, Apenas sandbox de filesystem  

## ADR-010: Formato de Serialização Interna
**Status**: Aceito  
**Data**: 2026-06-29  
**Contexto**: Precisamos de formato eficiente para comunicação com LLMs e armazenamento.  
**Decisão**: TOON (Token-Oriented Object Notation) para todas as estruturas de dados internas.  
**Consequências**:  
- Positivo: ~40% menos tokens que JSON, boa legibilidade, suporte a arrays tabulares  
- Negativo: Menos difundido que JSON, requer biblioteca de parsing  
**Alternativas consideradas**: JSON, YAML, MessagePack, Protocol Buffers  

## ADR-011: Engine de Logging
**Status**: Aceito  
**Data**: 2026-06-29  
**Contexto**: Precisamos de logging útil sem ruído desnecessário.  
**Decisão**: Biblioteca tracing com princípios de loggingsucks.com (structured, acionável, minimal).  
**Consequências**:  
- Positivo: Logs que são realmente úteis para debug, baixa overhead  
- Negativo: Requer mudança de mentalidade de logging tradicional  
**Alternativas consideradas**: logging padrão, structlog, zerolog  

## ADR-012: Framework para Desktop/Mobile
**Status**: Aceito  
**Data**: 2026-06-29  
**Contexto**: Precisamos de clientes nativos que sejam leves e seguros.  
**Decisão**: Tauri (Rust + WebView) para desktop e mobile.  
**Consequências**:  
- Positivo: Binários pequenos, segurança boa, reuse de código web, performance próxima do nativo  
- Negativo: Dependência de WebView do sistema, limitações de algumas APIs nativos  
**Alternativas consideradas**: Electron (pesado), Flutter (Dart), React Native (JS/native bridge)  

## ADR-013: Estratégia de Sandbox
**Status**: Aceito  
**Data**: 2026-06-29  
**Contexto**: Precisamos isolar operações potencialmente perigosas.  
**Decisão**: Combinação de filesystem jail (chroot/landlock) com opção de containers (devcontainers).  
**Consequências**:  
- Positivo: Segurança em camadas, performance quase nativa, flexibilidade para diferentes necessidades  
- Negativo: Complexidade de manter dois sistemas de isolamento  
**Alternativas consideradas**: Apenas filesystem isolation, Apenas containers, Nenhum isolamento (perigoso)  

## ADR-014: Sistema de Plugins/Extensões
**Status**: Pendente  
**Data**: 2026-06-29  
**Contexto**: Queremos permitir que a comunidade estenda funcionalidades.  
**Decisão**: Sistema de skills baseado em interface Rust com descoberta automática.  
**Consequências**:  
- Positivo: Ecossistema extensível, isolamento de código de terceiros  
- Negativo: Surface area de ataque aumentada, necessidade de versionamento cuidadoso  
**Alternativas consideradas**: Nenhuma extensibilidade, scripts externos, webhooks  

---
*Este documento será atualizado conforme novas decisões forem tomadas durante o desenvolvimento.*