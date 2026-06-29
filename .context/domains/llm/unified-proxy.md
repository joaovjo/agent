# Unified LLM Proxy

## Visão Geral
Proxy unificado e agnóstico para múltiplos providers de LLM. Permite trocar entre OpenAI, Anthropic, modelos locais e futuros providers sem alterar código do agente.

## Arquitetura
```
┌─────────────────┐
│  Pandow Agent   │
├─────────────────┤
│  LLM Proxy API  │  ← Interface unificada de completion
├─────────────────┤
│ ┌─────────────┐ │
│ │  Router     │ │  ← Seleciona provider baseado em config
│ └──────┬──────┘ │
│        │         │
│  ┌─────┼─────┐   │
│  │     │     │   │
│  ▼     ▼     ▼   │
│ OAI   Anth  Local│
└─────────────────┘
```

## Interface
```rust
#[async_trait]
pub trait LLMProvider: Send + Sync {
    fn name(&self) -> &str;
    fn capabilities(&self) -> Vec<Capability>;
    
    async fn complete(
        &self,
        request: CompletionRequest,
    ) -> Result<CompletionResponse>;
    
    async fn complete_stream(
        &self,
        request: CompletionRequest,
        on_token: Box<dyn Fn(&str) + Send>,
    ) -> Result<()>;
}

struct CompletionRequest {
    model: Option<String>,
    messages: Vec<Message>,
    system_prompt: Option<String>,
    temperature: Option<f32>,
    max_tokens: Option<u32>,
    tools: Vec<Tool>,
}
```

## Providers Suportados
### OpenAI
- Modelos: GPT-4o, GPT-4o-mini, o1, o3
- API: Chat Completions + Streaming
- Auth: API Key

### Anthropic
- Modelos: Claude 4 Sonnet, Claude 3.5 Haiku
- API: Messages API + Streaming
- Auth: API Key

### Local
- Ollama, llama.cpp, vLLM
- OpenAI-compatible API
- Auth: nenhuma ou token local

## Configuração
```json
{
  "llm": {
    "default_provider": "openai",
    "providers": {
      "openai": { "model": "gpt-4o", "api_key_env": "OPENAI_API_KEY" },
      "anthropic": { "model": "claude-sonnet-4-20250514", "api_key_env": "ANTHROPIC_API_KEY" },
      "local": { "base_url": "http://localhost:11434/v1", "model": "llama3" }
    },
    "routing": {
      "strategy": "cost", // "cost" | "capability" | "manual"
      "fallback": true
    }
  }
}
```

## Function Calling
- Providers têm formatos diferentes de function calling
- Proxy normaliza para formato interno unificado
- Converte de/para formato específico de cada provider

## Anti-Patterns
- Vendor lock-in em features específicas de um provider
- Não tratar erros específicos de cada provider
- Rate limiting sem retry/backoff
- Vazar API keys em logs