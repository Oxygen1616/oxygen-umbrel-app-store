# PegaProx for Umbrel

Community package in the [Oxygen app store](https://github.com/Oxygen1616/oxygen-umbrel-app-store)
for [PegaProx](https://github.com/PegaProx/project-pegaprox).

- App ID: `oxygen-pegaprox`
- Package version: `1.2.0-1`
- Upstream image: `ghcr.io/pegaprox/pegaprox:1.2.0`, pinned by digest
- Architectures: AMD64 and ARM64
- Open: `http://umbrel.local:5000`

Complete the initial setup in PegaProx. Umbrel's app proxy serves the web UI on
port 5000; the container publishes ports 5001 and 5002 for VNC and SSH consoles.
`PEGAPROX_BEHIND_PROXY=true` enables HTTP behind the proxy. Additional trusted
proxy and allowed-origin settings can be configured for your network using
PegaProx's documented environment variables.

PegaProx uses its embedded SQLite/SQLCipher database; no PostgreSQL service is
required. A one-shot permissions service uses the image's own `pegaprox` account
to prepare the bind mounts before startup.

Data is stored under `${APP_DATA_DIR}/data`:

| Directory | Container path | Contents |
| --- | --- | --- |
| `config` | `/app/config` | Configuration and database |
| `keys` | `/app/.config/pegaprox` | User-level encryption key storage |
| `logs` | `/app/logs` | Logs |
| `backups` | `/app/backups` | Backups |

Keep the key and database directories together when backing up or restoring.
This package is maintained by the community, independently of PegaProx and Umbrel.
