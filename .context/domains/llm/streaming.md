# LLM Streaming

## Visão Geral
Mecanismo de streaming de tokens do LLM para o cliente, permitindo feedback em tempo real durante a geração de resposta.

## Fluxo
```
LLM Provider → Token Stream → Proxy → Buffer → Client
                    │                            │
                    ▼                            ▼
               Rate limiting                SSE / IPC / WebSocket
```

## Server-Sent Events (SSE)
Formato padrão para streaming HTTP:
```
event: token
data: {"token": "refactor", "index": 42}

event: tool_call
data: {"name": "read_file", "args": {"path": "src/main.rs"}}

event: tool_result
data: {"name": "read_file", "result": "..."}

event: done
data: {}
```

## Implementação
```rust
enum StreamEvent {
    Token { token: String, index: u64 },
    ToolCall { name: String, args: Value, call_id: String },
    ToolResult { call_id: String, result: ToolResult },
    Error { code: String, message: String },
    Done,
}

struct StreamState {
    tokens_received: u64,
    tokens_per_second: f64,
    current_tool_call: Option<String>,
    buffer: String,
}
```

## Clientes
- **CLI**: Tokens printados em tempo real com cursor
- **TUI**: Tokens atualizados no painel de conversa
- **Desktop**: Tokens via IPC callback
- **Web**: SSE via EventSource ou fetch streaming
- **Mobile**: WebSocket com chunks pequenos

## Rate Limiting
- Tokens mostrados em tempo real mas throttled visualmente
- 60 fps máximo para UI
- Batch de tokens para eficiência de rede
- Buffer de ~50ms para agrupamento

## UX Considerations
- Mostrar indicador "pensando..." enquanto gera
- Tool calls visíveis como cartões expandíveis
- Resultados de tools mostrados inline
- Streaming interrompível (CTRL+C)

## Anti-Patterns
- Mostrar tokens muito rápido (flickering)
- Buffer muito grande (delay perceptível)
- Não mostrar tool calls em andamento
- Streaming sem fallback para não-streaming