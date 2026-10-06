# oxygen-odysseus

This is a community Umbrel app package for [Odysseus](https://odysseus-dev.github.io/odysseus/), a self-hosted AI workspace.

## Package Details

- **App ID**: oxygen-odysseus
- **Version**: 1.0.0
- **Category**: ai
- **Port**: 7000
- **Submitter**: Oxygen1616

## Description

Odysseus is a self-hosted AI workspace for chat, agents, research, documents, email, notes, calendar, and local model workflows. It features chat with local/API models, agents with tools, deep research, document editing, email management, notes and tasks with calendar sync, and extras like image editor, themes, and web search.

### Key Features

- **Chat + Agents** — local/API models, tools, MCP, files, shell, skills, and memory
- **Cookbook** — hardware-aware model recommendations, downloads, and serving
- **Deep Research** — multi-step web research with source reading and report generation
- **Compare** — blind side-by-side model testing and synthesis
- **Documents** — writing-first editor with AI edits, suggestions, Markdown, HTML, CSV, and syntax highlighting
- **Email** — IMAP/SMTP inbox with triage, tags, summaries, reminders, and reply drafts
- **Notes, Tasks + Calendar** — reminders, todos, scheduled agent tasks, and CalDAV sync
- **Extras** — gallery/image editor, themes, uploads, web search, presets, sessions, and 2FA

### Ollama Integration

Odysseus includes native support for [Ollama](https://ollama.ai/), allowing you to run local LLMs directly. The app connects to Ollama at http://host.docker.internal:11434/v1 by default, enabling:

- Use of any model installed via Ollama
- No need for API keys for local models
- Automatic model discovery from Ollama's registry

### Custom OpenAI Endpoints

Odysseus also supports custom OpenAI-compatible endpoints. Set the OPENAI_API_KEY and OPENAI_BASE_URL environment variables to point to your own OpenAI-compatible API instance.

### Configuration

After installation, access Odysseus at http://umbrel.local:7000.

### Environment Variables

- ODYSSEUS_OLLAMA_MODEL - Default Ollama model (default: gemma:2b)
- ODYSSEUS_OPENAI_API_KEY - OpenAI API key for remote models
- ODYSSEUS_OPENAI_BASE_URL - Custom OpenAI-compatible endpoint base URL
- ODYSSEUS_AUTH_ENABLED - Enable authentication (default: true)
- ODYSSEUS_LOCALHOST_BYPASS - Bypass localhost check (default: false)
- ODYSSEUS_SECRET_KEY - Django secret key
- ODYSSEUS_JWT_SECRET_KEY - JWT secret key

### Data Persistence

All user data is persisted under ${APP_DATA_DIR}/data/app/odysseus/. This includes:

- Config directory with application settings
- Logs directory
- Backups directory
- Ollama model data (separate volume at ${APP_DATA_DIR}/data/ollama)

To relocate app data, use the Umbrel dashboard under Settings → Apps → Odysseus → Move App Data.

### Backups

Users can enable backups in the Umbrel dashboard. Included in backups:

- data/app/odysseus/config/ - Application configuration
- data/app/odysseus/logs/ - Application logs
- data/app/odysseus/backups/ - User-created backups

Ollama models are stored separately at ${APP_DATA_DIR}/data/ollama/ and are included in system backups.

### Updates

Updates are handled through the Umbrel App Store. The package update flow copies docker-compose.yml, umbrel-app.yml, exports.sh, top-level *.template files, and hooks/ scripts from the package into installed app data. User data is preserved across updates.

### Supported Architectures

- linux/amd64 - x86_64 PCs and servers
- linux/arm64 - 64-bit ARM devices (Raspberry Pi 4/5, etc.)

### Support

- **Source**: https://github.com/odysseus-dev/odysseus
- **Community Store**: https://github.com/Oxygen1616/oxygen-umbrel-app-store
- **Issues**: https://github.com/odysseus-dev/odysseus/issues

## License

Odysseus is licensed under AGPL-3.0-or-later. See the LICENSE file for details.

The Umbrel app package is a community contribution and is not officially supported by the Odysseus development team or the Umbrel company.

---
*This package was created by the Oxygen1616 community and is maintained independently. It is not affiliated with or endorsed by the official Umbrel team or the Odysseus development team.*
