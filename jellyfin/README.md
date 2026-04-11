# Jellyfin

Media server running on Docker on Proxmox.

## Setup

- iGPU passthrough enabled on Proxmox VM for QuickSync hardware transcoding
- Media files stored at `/data` (Samba share)
- Config stored alongside compose file

## Services

- **Jellyfin** — media server with hardware transcoding via Intel QuickSync

## Restore

1. Spin up Ubuntu VM on Proxmox with iGPU passthrough enabled
2. Install Docker
3. Clone this repo
4. Create `/docker/jellyfin/` and copy files there
5. `docker compose up -d`
6. Reconfigure Jellyfin through web UI at port 8096
7. Point libraries at `/data`

## Notes

- Hardware transcoding requires iGPU passthrough in Proxmox (hostpci0: 0000:00:02)
- VM needs 16GB RAM allocated due to transcoding spikes
- QuickSync handles transcoding with near-zero CPU usage
