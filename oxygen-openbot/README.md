# OpenBot on Umbrel OS

[OpenBot](https://github.com/nightly-labs/openbot) is a local-first workspace and self-hosted server for persistent AI teammates and multi-agent coordination.

It allows you to run autonomous agents (Claude Code, Codex, Grok, OpenCode, Hermes Agent, Antigravity, and custom ACP/MCP tools) connected to your private workspaces, with all state, conversation history, and SQLite databases stored locally on your Umbrel node.

---

## Features

- **Multi-Agent Orchestration**: Seamlessly coordinate diverse AI agents across dedicated team channels.
- **Local Sovereignty**: All code, files, chats, SQLite database, and cryptographic keyrings stay on your Umbrel storage.
- **Model Context Protocol (MCP)**: Native integration for MCP servers and tools.
- **Agent Client Protocol (ACP)**: Run background and scheduled agent routines with isolated container execution.
- **Cross-Platform Access**: Access your self-hosted Umbrel node from [openbot.run/app](https://openbot.run/app), the OpenBot Desktop app (macOS, Windows, Linux), or mobile apps (iOS & Android).

---

## Quick Setup Guide

### 1. Web Management Dashboard
Once installed, open OpenBot from your Umbrel dashboard (`http://umbrel.local:8090`). The dashboard provides live status, pairing commands, and direct links to the web and desktop apps.

### 2. Sign In Your Umbrel Server
OpenBot connects securely outbound to your account via WebRTC/TURN. To pair your Umbrel instance with your OpenBot account, run the following in your Umbrel SSH/terminal:

```bash
docker exec -it oxygen-openbot_openbot_1 openbot login
```
*Follow the prompt to enter your email and verification code.*

For automated setups or remote server claims, use:
```bash
docker exec -it oxygen-openbot_openbot_1 openbot login --claim
```

### 3. Connect Mobile App (iOS / Android)
Generate an on-screen pairing QR code for the OpenBot mobile app:

```bash
docker exec -it oxygen-openbot_openbot_1 openbot mobile-connect
```

### 4. Check Server Status
Inspect your agent processes, virtual display status, and active channels:

```bash
docker exec -it oxygen-openbot_openbot_1 openbot status
```

---

## Technical Details

- **Docker Image**: `ghcr.io/nightly-labs/openbot:latest` (multi-arch `linux/amd64` and `linux/arm64`)
- **Web Dashboard**: Port `8090` (proxied to Nginx internal port `80`)
- **Data Persistence**: `${APP_DATA_DIR}/data/openbot` mounted to container `/data`
- **User & Permissions**: The container runs under unprivileged UID `1000`. An init `permissions` container manages proper file ownership.
- **Sandbox Profile**: Configured with `seccomp=./seccomp.json` to allow Electron user namespaces while maintaining sandbox isolation.

---

## Upstream Links

- **Repository**: [nightly-labs/openbot](https://github.com/nightly-labs/openbot)
- **Official Documentation**: [OpenBot Docs](https://github.com/nightly-labs/openbot/tree/main/docs)
- **Web App**: [openbot.run/app](https://openbot.run/app)
- **Releases**: [GitHub Releases](https://github.com/nightly-labs/openbot/releases)
