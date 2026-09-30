# Hindsight provider configuration

Configure the `server.environment` entries in `docker-compose.yml`, then recreate
the server container to apply changes. On Umbrel, edit the installed app's Compose
file. When running Compose directly, these variables can also come from the shell
or an explicitly supplied `--env-file` (a `.env` file only works if the launcher
loads it).

Set `HINDSIGHT_API_LLM_PROVIDER` to the provider name and
`HINDSIGHT_API_LLM_API_KEY` to its API key when required. Set
`HINDSIGHT_API_LLM_MODEL` to override its default model. Leave the model and base
URL entries without `=...` to use provider defaults when the variables are unset.
OpenAI is the default provider and requires a real API key before use.

For example, replace the four environment entries with one of these configurations:

```yaml
# Hosted provider (uses Anthropic's default model and endpoint)
- HINDSIGHT_API_LLM_PROVIDER=anthropic
- HINDSIGHT_API_LLM_API_KEY=your-anthropic-api-key
```

```yaml
# Any custom OpenAI-compatible Chat Completions endpoint
- HINDSIGHT_API_LLM_PROVIDER=openai
- HINDSIGHT_API_LLM_API_KEY=your-provider-api-key
- HINDSIGHT_API_LLM_MODEL=your-model-id
- HINDSIGHT_API_LLM_BASE_URL=https://your-provider.example/v1
```

```yaml
# Ollama running on the Umbrel host (model must already be available)
- HINDSIGHT_API_LLM_PROVIDER=ollama
- HINDSIGHT_API_LLM_MODEL=llama3
- HINDSIGHT_API_LLM_BASE_URL=http://host.docker.internal:11434/v1
```

For LM Studio, use `lmstudio`, its loaded model ID, and the server's reachable
base URL (normally port `1234` with `/v1`). Local servers must listen on an
interface reachable from the container. Use a LAN address for another machine;
`localhost` inside the container refers to Hindsight itself.

Other provider names can be selected the same way. Providers such as Bedrock,
Vertex AI, and LiteLLM Router require additional provider-specific environment
entries or credential mounts. Subscription providers (`openai-codex`,
`claude-code`, `cursor`, `github-copilot`) require their documented authentication
and runtime dependencies inside the container; selecting the name alone does not
connect a host login. The stock image does not include the built-in `llamacpp`
runtime, which requires a custom image.

See the [upstream provider setup instructions](https://hindsight.vectorize.io/developer/models)
for provider names, credentials, dependencies, and endpoint formats. Keep real
credentials out of version control.
