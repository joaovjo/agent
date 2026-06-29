# Tool System

## Visão Geral
Sistema de ferramentas que o agente usa para interagir com o ambiente de desenvolvimento. Cada ferramenta é uma operação atômica com entrada/saída tipadas.

## Catálogo de Ferramentas

### File Operations
- `read_file(path, offset?, limit?) → content`
- `write_file(path, content) → ok`
- `edit_file(path, old_string, new_string) → ok`
- `glob(pattern, path?) → files[]`
- `grep(pattern, include?, path?) → matches[]`

### Process Operations
- `run_command(cmd, args[], workdir?, timeout?, env?) → {stdout, stderr, exit_code}`
- `run_pty(cmd, args[], workdir?, on_output?) → process_handle`

### Context Operations
- `read_context(path?) → content`
- `update_context(path, content) → ok`
- `search_context(query) → results[]`

### LLM Operations
- `llm_complete(prompt, model?, system?) → response`
- `llm_stream(prompt, on_token?) → stream`

### Security Operations
- `check_permission(action, path?) → approved/denied`
- `request_escalation(action, reason) → user_decision`

## Design
- Cada ferramenta implementa o trait `Tool`:
```rust
#[async_trait]
pub trait Tool: Send + Sync {
    fn name(&self) -> &str;
    fn description(&self) -> &str;
    fn parameters(&self) -> Vec<Parameter>;
    async fn execute(&self, params: HashMap<String, Value>) -> Result<ToolResult>;
}
```
- Ferramentas são registradas em um `ToolRegistry`
- O LLM seleciona ferramentas via function calling
- Resultados são retornados como contexto para o LLM

## Extensibilidade
- Plugins podem registrar novas ferramentas
- Ferramentas podem ser desativadas por segurança
- Rate limiting por ferramenta
- Logging de todas as invocações

## Anti-Patterns
- Ferramentas monolíticas (muitas responsabilidades)
- Parâmetros não validados
- Falta de timeout
- Não reportar erros adequadamente
- Execução sem verificação de permissão