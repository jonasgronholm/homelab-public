# Servarr Stack

Media management stack running on a Docker VM on Proxmox.

## Services

- **Gluetun** — VPN container (ProtonVPN WireGuard), all download traffic routed through it
- **qBittorrent** — torrent client, routed through Gluetun
- **NZBGet** — usenet client, routed through Gluetun
- **Prowlarr** — indexer manager
- **Sonarr** — TV show management
- **Radarr** — movie management
- **Lidarr** — music management
- **Bazarr** — subtitle management
- **Seerr** — media request management formerly known as jellyseerr
- **Deunhealth** — restarts unhealthy containers automatically

## Restore

1. Spin up Ubuntu VM on Proxmox
2. Install Docker
3. Clone this repo
4. Create `/docker/servarr/` and copy files there
5. Copy `.env.example` to `.env` and fill in values from Proton Pass
6. `docker compose up -d`

## Notes

- Media files stored at `/data`
- Config files stored alongside compose file
- Download clients route through Gluetun VPN
- VPN exit country: Sweden
