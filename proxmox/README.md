# Proxmox

Proxmox VE 8.x running on dedicated server.

## Hardware

- **CPU:** Intel i5-14600K (20 cores, QuickSync iGPU)
- **RAM:** 32GB
- **Network:** Managed via UniFi UDM

## Virtual Machines & Containers

| ID  | Name           | Type | CPU | RAM   | Notes                    |
|-----|----------------|------|-----|-------|--------------------------|
| 100 | Home-Assistant | QEMU | 2   | 4GB   |HA with Google Drive backup|
| 101 | Samba          | LXC  | 2   | 2GB   | File share server        |
| 102 | servarr        | QEMU | 8   | 16GB  | Jellyfin + arr stack     |

## Notable Configuration

- iGPU passthrough (hostpci0: 0000:00:02) on servarr VM for Jellyfin QuickSync
- Servarr VM disk: 64GB local-lvm
- Backups: not configured, restore from scratch using this repo
- Proxmox VE integration in Home Assistant used for monitoring Samba/ZFS disk space

## Network

- Proxmox on server VLAN managed via UniFi UDM
- Cloudflare Tunnel for external Home Assistant access (country restricted)

## Storage

- **local** — Proxmox OS and config
- **local-lvm** — VM disks
- **tank** — ZFS pool (1.93TB) for media storage, mounted into Samba LXC
  - Shared via Samba to the rest of the network
  - Disk space monitored via Proxmox VE integration in Home Assistant
  - No redundancy — media is redownloadable via arr stack if disk fails
  - Future plan: add second disk for ZFS mirror when prices drop
