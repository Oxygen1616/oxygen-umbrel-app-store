# AI Memory for umbrelOS

Runs [ai-memory](https://github.com/akitaonrails/ai-memory) 3.1.1 from the public
`akitaonrails/ai-memory:3.1.1` Docker Hub image (amd64).

## Access

- MCP endpoint: `http://umbrel.local:49374/mcp`
- Auth: every request needs `Authorization: Bearer <password>`. The password is
  `APP_PASSWORD`, generated per install and shown on the Umbrel app page.
- Host allowlist: `${DEVICE_DOMAIN_NAME}`, `localhost`, `127.0.0.1` and the
  container name `oxygen-ai-memory_server_1`. Requests with other Host headers
  are rejected by ai-memory.

## Connect a client

```bash
ai-memory install-mcp --client claude-code --server-url http://umbrel.local:49374/mcp --auth-token <password> --apply
```

Supported clients are listed in the upstream support matrix. For Hermes Agent and
OpenClaw, use the same `--client` names shown by `ai-memory install-mcp --help`.

## LLM provider (optional)

Without an LLM provider, ai-memory uses rule-based summaries. To use Ollama or
another OpenAI-compatible server, uncomment the `AI_MEMORY_LLM_*` lines in
`docker-compose.yml` and point the base URL at that app's port on Umbrel.

## Data

State is stored in the app's data directory (`/data` in the container). Take
backups with `ai-memory backup` rather than copying the live SQLite file.
