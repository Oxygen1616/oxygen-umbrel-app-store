# oxygen-pegaprox

This is a community Umbrel app package for [PegaProx](https://pegaprox.com), a powerful web-based management interface for Proxmox VE and XCP-ng clusters.

## Package Details

- **App ID**: oxygen-pegaprox
- **Version**: 1.2.0
- **Category**: networking
- **Port**: 5000
- **Submitter**: Oxygen1616

## Description

PegaProx is a powerful web-based management interface for Proxmox VE and XCP-ng clusters. Manage multiple clusters from a single dashboard with features like live monitoring, VM management, automated tasks, and more.

### Features

- Multi-Cluster Management with unified dashboard
- Live Metrics monitoring via SSE
- Live VM Migration between nodes
- Cross-Cluster Load Balancing
- Cross-Hypervisor Migration (ESXi, Proxmox VE, XCP-ng)
- VM & Container Management with quick actions
- Snapshots and Snapshot Replication
- Backups with Backup Verification
- noVNC / xterm.js Console access
- Role-based Access Control (Admin, Operator, Viewer)
- API Token Management
- 2FA Authentication (TOTP, WebAuthn/FIDO2)
- LDAP / OIDC integration
- Multi-Tenancy support
- IP Whitelisting / Blacklisting
- Full-DB Encryption with SQLCipher
- Scheduled Tasks and Snapshot Schedules
- Rolling Node Updates
- Alerts and Audit Logging
- Audit Search with full-text filtering
- SIEM Forwarder for audit events
- Config Drift Detection
- CIS Hardening one-click audit
- Cost Dashboard / Chargeback
- Power & Carbon Tracking
- Insights with right-sizing recommendations
- Network Topology Visualization (SVG graph)
- Compliance Dashboard (BSI Grundschutz, ISO 27001, NIS2, SOC 2)
- CVE Reporting
- Cloud-Init Template Library
- Site Recovery with DR plans
- Backup SLA Tracking
- ZFS / Cross-Cluster Replication
- V2P / ESXi Migration
- Webhook Channels (Slack, Discord, Microsoft Teams, ntfy)
- Web Push Notifications
- PWA / Installable App
- Client Portal plugin
- Public Status Page
- Notifications Plugin (ntfy + Apprise integration)
- Docker Swarm Manager plugin
- TrueNAS SCALE Manager plugin
- Plugin Config Editor
- Offline Mode (air-gap)
- 17 Themes (Dark/Light, Proxmox, Corporate, etc.)
- Corporate Layout with tree-based sidebar
- Multi-Language support (12 languages)
- Responsive + PWA design
- PBS Integration with verification
- Prometheus Exporter
- Pre-built Grafana Dashboards

## Installation

This package is available in the [Oxygen1616 Community Umbrel App Store](https://github.com/Oxygen1616/oxygen-umbrel-app-store).

To install:
1. Open your Umbrel dashboard
2. Go to the App Store
3. Search for "PegaProx" or browse the community apps
4. Click "Install"
5. Follow the setup flow at http://umbrel.local:5000

## Configuration

After installation, access PegaProx at http://umbrel.local:5000.

### Default Credentials

- **First Login**: Create your admin account during the setup flow
- **Admin Portal**: Settings > About after initial setup

### Environment Variables

The following Umbrel environment variables are used:

- PEGAPROX_DB_PASSWORD - PostgreSQL password for the app database
- PEGAPROX_SECRET_KEY - Django secret key for the application
- PEGAPROX_JWT_SECRET_KEY - JWT secret key for API authentication
- DEVICE_DOMAIN_NAME - Umbrel .local domain name
- APP_PROXY_PORT - Manifest port (default: 5000)

### Data Persistence

All user data is persisted under ${APP_DATA_DIR}/data/app/pegaprox/. This includes:

- Database: postgres service data
- Config: server service config directory
- Logs: server service log directory
- Backups: server service backup directory

To relocate app data, use the Umbrel dashboard under Settings → Apps → PegaProx → Move App Data.

### Backups

Users can enable backups in the Umbrel dashboard. The following directories are included in backups by default:

- data/app/pegaprox/db/ - PostgreSQL database
- data/app/pegaprox/config/ - Server configuration
- data/app/pegaprox/logs/ - Application logs
- data/app/pegaprox/backups/ - User-created backups

To exclude specific files or directories from backups, add them to the ackupIgnore list in the umbrel-app.yml.

### Updates

Updates are handled through the Umbrel App Store. The package update flow copies the following files from the package into installed app data:

- docker-compose.yml
- umbrel-app.yml
- exports.sh (if present)
- Top-level *.template files
- hooks/ scripts

Other user data is preserved across updates.

## Supported Architectures

- linux/amd64 - x86_64 PCs and servers
- linux/arm64 - 64-bit ARM devices (Raspberry Pi 4/5, etc.)

## Support

- **Source**: https://github.com/PegaProx/project-pegaprox
- **Community Store**: https://github.com/Oxygen1616/oxygen-umbrel-app-store
- **Issues**: https://github.com/PegaProx/project-pegaprox/issues
- **Discord**: PegaProx Community Server

## License

PegaProx is licensed under the AGPL-3.0. See the NOTICE file for attribution requirements.

The Umbrel app package is a community contribution and is not officially supported by the PegaProx team or the Umbrel company.

---
*This package was created by the Oxygen1616 community and is maintained independently. It is not affiliated with or endorsed by the official Umbrel team.*
