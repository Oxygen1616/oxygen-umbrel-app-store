# AutoClip for Umbrel OS

Turn any video, link, or live stream into ready-to-post clips using AI highlight detection, smart 9:16 reframing, burned-in captions, and cover generation.

- **Upstream Repository:** https://github.com/zhouxiaoka/autoclip
- **Umbrel Port:** `8796` (`app_proxy` -> `server:8000`)
- **Data Persistence:** `${APP_DATA_DIR}/data/storage` (SQLite DB, projects, uploads, outputs, logs) and `${APP_DATA_DIR}/data/redis`

## Configuration
Open **Settings** in the AutoClip web UI to configure your LLM provider API key (OpenAI, Gemini, DashScope, DeepSeek, etc.) or local Ollama endpoint.
