# Odysseus for Umbrel

Community package in the [Oxygen app store](https://github.com/Oxygen1616/oxygen-umbrel-app-store)
for [Odysseus](https://github.com/odysseus-dev/odysseus).

- App ID: `oxygen-odysseus`
- Package version: `1.0.2-1`
- Upstream image: `ghcr.io/odysseus-dev/odysseus:1.0.2`, pinned by digest
- Architectures: AMD64 and ARM64
- Open: `http://umbrel.local:7000`
- Initial username: `admin`; password: the app password displayed by Umbrel

The image's entrypoint prepares storage and starts the application as UID/GID
1000. Do not force `user: 1000:1000` in Compose: the entrypoint needs to set up
the account and repair bind-mount permissions first.

The bundled Ollama service is reachable from Odysseus at `http://ollama:11434`.
It uses the CPU and starts without downloaded models. Select/download a model
in the app, or configure an external API provider or OpenAI-compatible endpoint
in Odysseus. For a server running on the Umbrel host, use
`http://host.docker.internal:<port>`.

Ollama has no published host port, so it can coexist with other Ollama apps.
Web search and other integrations may need a separately configured service
(such as SearXNG or ChromaDB) or API credentials; they are not bundled here.

Data is stored under `${APP_DATA_DIR}/data`:

| Directory | Container path | Contents |
| --- | --- | --- |
| `odysseus` | `/app/data` | Database, settings, documents |
| `logs` | `/app/logs` | Logs |
| `ssh` | `/app/.ssh` | Remote model-server SSH identities |
| `huggingface` | `/app/.cache/huggingface` | Model cache |
| `local` | `/app/.local` | Installed local tools and packages |
| `ollama` | `/root/.ollama` in Ollama | Ollama models |

The `ALLOWED_ORIGINS` setting uses Umbrel's device domain on port 7000. Add your
origin in the app's environment settings if you access it through a different
domain or IP address. Model caches can be large; account for them in backups.

This package is maintained by the community, independently of Odysseus and Umbrel.
