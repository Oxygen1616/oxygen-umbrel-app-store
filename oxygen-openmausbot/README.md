# OpenMausBot on Umbrel OS

[OpenMausBot](https://github.com/milind-soni/OpenMausBot) (also known as **MausBot**, formerly **OpenGrokBot**) is the open-source Grok Bot alternative: your own self-hosted team of AI bots inside a modern, local-first chat workspace.

It is an open-source alternative to **Grok Bot**, **Meta Muse**, **OpenAI dots**, and **Cue by Manus** — bring-your-own-agent, local-first, on the models and keys you already have.

---

## Highlights

- **Persistent AI Teammates**: Every bot in your sidebar is a full agent (Claude Code, Codex, Grok, or custom ACP/MCP tools) with its own memory, personality, and assigned role.
- **Local Sovereignty**: All conversation history, SQLite databases, files, and transcripts stay 100% on your Umbrel node.
- **Universal Multi-Engine Support**: Connect Anthropic Claude, OpenAI Codex, xAI Grok, or local models.
- **Cross-Platform Access**: Connect from the OpenMausBot desktop app (macOS, Windows, Ubuntu) or mobile app via instant QR code pairing.
- **Tool & App Integrations**: Native Model Context Protocol (MCP) and agent browser automation.

---

## Quick Setup & Pairing

### 1. Web Access
Once installed, open OpenMausBot from your Umbrel dashboard (`http://umbrel.local:8799`).

### 2. Pair Desktop App or Mobile App
To generate a pairing code for the OpenMausBot desktop or mobile app, run via SSH/terminal:

```bash
docker exec -it oxygen-openmausbot_server_1 /data/bin/bin/openmausbot pair
```

- **Desktop app**: Click **＋ → Connect to a server**, then paste the printed pairing link.
- **Mobile app**: Scan the printed ASCII QR code with your mobile camera or app.

### 3. Add AI Engine API Keys
You can set engine keys via terminal or directly inside the app's Settings:

```bash
# Set Claude API key
docker exec -it oxygen-openmausbot_server_1 /data/bin/bin/openmausbot engine set claude --key-env ANTHROPIC_API_KEY
```

### 4. Health & Diagnostics
Check the status of your running bots, engines, and database:

```bash
docker exec -it oxygen-openmausbot_server_1 /data/bin/bin/openmausbot doctor
```

---

## Technical Details

- **Port**: `8799` (proxied by Umbrel's `app_proxy`)
- **Runtime**: Node.js 24 Bookworm
- **Data Directory**: `${APP_DATA_DIR}/data/openmausbot` (persistent across restarts and updates)
- **Binary Directory**: `${APP_DATA_DIR}/data/bin` (cached persistent npm binaries)

---

## Upstream Links

- **Repository**: [milind-soni/OpenMausBot](https://github.com/milind-soni/OpenMausBot)
- **Website**: [openmausbot.com](https://openmausbot.com)
- **Issues & Support**: [GitHub Issues](https://github.com/milind-soni/OpenMausBot/issues)
