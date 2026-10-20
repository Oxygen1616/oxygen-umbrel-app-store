# AI Memory for umbrelOS

Runs [ai-memory](https://github.com/akitaonrails/ai-memory) 3.1.1 from the public
`akitaonrails/ai-memory:3.1.1` Docker Hub image.

The MCP HTTP endpoint is served at `/mcp` on the app's port (49374). Connect Claude
Code or another MCP HTTP client to it. State is stored in the app's data directory
and survives restarts.

The image's own LLM provider settings are not configured by this app. See the
upstream repository for the provider options if you need them.
