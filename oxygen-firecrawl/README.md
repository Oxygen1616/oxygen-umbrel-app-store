# Firecrawl for Umbrel OS

Self-hosted Firecrawl API for turning websites into LLM-ready markdown and structured JSON.

- **Upstream Repository:** https://github.com/firecrawl/firecrawl
- **Umbrel Port:** `8797` (`app_proxy` -> `api:3002`)
- **Services:** `api`, `playwright-service`, `nuq-postgres`, `redis`, `rabbitmq`

## Quick Test
```bash
curl -X POST http://umbrel.local:8797/v1/scrape \
  -H "Content-Type: application/json" \
  -d '{"url":"https://example.com","formats":["markdown"]}'
```
