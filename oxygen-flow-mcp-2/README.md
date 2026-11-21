# Flow MCP 2 for umbrelOS

**Flow MCP 2** is an automated video generation, REST API, and Model Context Protocol (MCP) server for Google Flow (Instance 2).

### Features
* Automates image, thumbnail, and video shot generation from text or image prompts.
* Connects directly to Umbrel's Brave 2 browser (`oxygen-brave-2`) via Chrome DevTools Protocol (CDP on port 9224).
* Drives Google Flow concurrently for a second Google account using free daily credits (50/day) or paid Google plans.
* Generates studio-quality Google Gemini TTS narration audio.
* Assembles multi-scene videos with background music, audio ducking, and sound mixing.
* Generates and burns dynamic kinetic animated captions/subtitles (Shorts/TikTok style).
* Exposes a Web UI (**Flow Studio**) and REST API endpoints on port `8788`.
* Fully compatible with n8n, Claude Code, Antigravity, and MCP-compatible AI agents via `/mcp`.
