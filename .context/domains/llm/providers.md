# LLM Providers

## OpenAI
| Feature | Suporte | Notas |
|---------|---------|-------|
| Chat Completions | ✅ | API principal |
| Streaming | ✅ | SSE |
| Function Calling | ✅ | Parallel function calls |
| Vision | ✅ | GPT-4o |
| Structured Output | ✅ | JSON Schema |
| Responses API | ✅ | Novo formato |
| Rate Limits | Tier-based | 10k RPM (Tier 5) |

**Modelos recomendados**:
- Principal: `gpt-4o` (velocidade + qualidade)
- Econômico: `gpt-4o-mini`
- Raciocínio: `o1`, `o3` (problemas complexos)

## Anthropic
| Feature | Suporte | Notas |
|---------|---------|-------|
| Messages API | ✅ | API principal |
| Streaming | ✅ | SSE |
| Tool Use | ✅ | Function calling nativo |
| Extended Thinking | ✅ | Raciocínio visível |
| Vision | ✅ | Claude 3+ |
| Context caching | ✅ | Reduz custo em prompts longos |
| Rate Limits | Usage-based | 5k requests/min (Tier 4) |

**Modelos recomendados**:
- Principal: `claude-sonnet-4-20250514`
- Rápido: `claude-3-5-haiku-20241022`
- Raciocínio: `claude-opus-4-20250514` (quando disponível)

## Modelos Locais
| Engine | Protocolo | Performance | GPU |
|--------|-----------|-------------|-----|
| Ollama | OpenAI-compat | ★★★ | Required |
| llama.cpp | OpenAI-compat | ★★★★ | Optional (CPU ok) |
| vLLM | OpenAI-compat | ★★★★★ | Required |
| LM Studio | OpenAI-compat | ★★★ | Required |

**Modelos recomendados (local)**:
- Código: DeepSeek Coder V2, CodeLlama, Qwen2.5-Coder
- Geral: Llama 3, Mistral, Qwen 2.5
- Raciocínio: DeepSeek R1 (destilado)

## Fallback Strategy
```
1. Tenta provider principal
2. Se falhar (rate limit, timeout, outage):
   a. Retry com backoff exponencial (3x)
   b. Fallback para próximo provider na lista
   c. Se todos falharem, reporta erro ao usuário
```

## Cost Optimization
| Estratégia | Descrição | Economia |
|------------|-----------|----------|
| Model routing | Usa modelo barato para tarefas simples | 40-60% |
| Context caching | Reutiliza contexto frequente | Até 90% |
| Prompt compression | Remove boilerplate | 20-30% |
| Batch processing | Agrupa requests similares | 10-20% |