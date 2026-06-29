# Orchestration Workflow

## Message Bus Protocol

### gRPC Stream Definitions
```protobuf
service AgentOrchestrator {
  rpc DelegateTask(TaskRequest) returns (stream TaskUpdate);
  rpc ValidateOutcome(ValidationRequest) returns (ValidationResponse);
  rpc RegisterAgent(AgentInfo) returns (AgentStatus);
  rpc Heartbeat(HeartbeatRequest) returns (HeartbeatResponse);
}

message TaskRequest {
  string task_id = 1;
  string role = 2;          // target agent role
  string objective = 3;     // what to accomplish
  map<string, string> context = 4;
  repeated string dependencies = 5;
  int32 priority = 6;
  bool blocking = 7;
}

message TaskUpdate {
  string task_id = 1;
  enum Status { STARTED, IN_PROGRESS, COMPLETED, FAILED, BLOCKED }
  Status status = 2;
  string output = 3;
  repeated string artifacts = 4;
  int32 progress_percent = 5;
}
```

## Sprint Planning Workflow

### Fase 1: Objetivo → Issues
1. Usuário fornece objetivo alto nível
2. Orchestrator usa skill `grill-with-docs` para alinhar
3. Cria issues estruturadas (T-001, T-002, etc.)
4. Define dependências e ordem

### Fase 2: Issues → Delegação
1. Para cada issue:
   - Orchestrator seleciona agente apropriado
   - Envia TaskRequest via gRPC stream
   - Monitora progresso via TaskUpdate

### Fase 3: Execução → Validação
1. Agente executa, produz artifacts
2. Orchestrator valida outcome contra criteria
3. Se falhar: feedback loop, re-delega
4. Se passar: marca complete, atualiza dependências

### Fase 4: Integração → Handoff
1. Merge de mudanças (via git)
2. Testes de integração
3. Handoff para próxima fase
4. Atualização de .context/

## Handoff Template

```markdown
# Handoff: [Task ID] - [Objective]

## Contexto
- **Objetivo original**: ...
- **Agente responsável**: ...
- **Tempo total**: ...

## Decisões Tomadas
| Decisão | Racional | Tradeoffs |
|---------|----------|-----------|
| ... | ... | ... |

## Artifacts Produzidos
- [ ] Código: `path/to/file.rs`
- [ ] Testes: `path/to/test.rs`
- [ ] Docs: `path/to/doc.md`
- [ ] Config: `path/to/config.toml`

## Próximos Passos
1. ...
2. ...

## Blockers/Riscos
- ...

## Lições Aprendidas
- O que funcionou bem
- O que evitar na próxima vez
```

## Anti-Patterns (19 do ai-team + novos)

1. **Sprint sem objective claro** — sempre ter OKR
2. **Delegar sem contexto** — sempre incluir full context
3. **Pular validação** — nunca assumir que funcionou
4. **Merge sem testes** — CI/CD obrigatório
5. **Documentação desatualizada** — .context/ sempre sincronizado
6. **Agent sprawl** — limitar agentes simultâneos (max 5)
7. **Context overflow** — compaction automática, resumo
7. **Handoff incompleto** — template obrigatório
8. **Silent failures** — logging explícito
9. **Over-engineering** — YAGNI enforced
10. **Security bypass** — deny-by-default sempre
11. **No rollback plan** — sempre ter plano B
12. **Single point of failure** — conhecimento distribuído
13. **Inconsistent naming** — convenções enforcadas
14. **Untested assumptions** — validar antes de codificar
15. **Scope creep** — change control process
16. **Knowledge silos** — cross-training obrigatório
17. **Tool misuse** — treinamento antes de uso
18. **Burnout pace** — sustainable velocity
19. **Metric gaming** — medir outcomes, não outputs

## Novos Anti-Patterns
20. **Model switching without reason** — consistency matters
21. **Ignoring .context/** — memory is core
22. **Hardcoding secrets** — zero tolerance
23. **Skipping architecture review** — ADR required