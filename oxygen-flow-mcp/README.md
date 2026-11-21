# Flow MCP for umbrelOS

**Flow MCP** is an automated video generation, REST API, and Model Context Protocol (MCP) server for Google Flow.

### Features
* Automates image, thumbnail, and video shot generation from text or image prompts.
* Connects directly to Umbrel's Brave browser (`oxygen-brave`) via Chrome DevTools Protocol (CDP on port 9223).
* Drives Google Flow using your account's daily free credits (50/day) or paid Google plan.
* Generates studio-quality Google Gemini TTS narration audio.
* Assembles multi-scene videos with background music, audio ducking, and sound mixing.
* Generates and burns dynamic kinetic animated captions/subtitles (Shorts/TikTok style).
* Exposes a Web UI (**Flow Studio**) and REST API endpoints on port `8787`.
* Fully compatible with n8n, Claude Code, Antigravity, and MCP-compatible AI agents via `/mcp`.
