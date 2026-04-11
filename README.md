# Homelab

Personal homelab built and maintained as a hands-on learning environment for cloud-native and DevOps technologies. All configuration is version-controlled and deployed as code.

## Technologies

- **Orchestration:** Kubernetes (k3s), Docker, Docker Compose
- **Ingress & SSL:** Traefik, Let's Encrypt, Cloudflare DNS challenge
- **Virtualisation:** Proxmox VE
- **Networking:** UniFi, WireGuard VPN, Cloudflare Tunnel
- **GitOps:** Flux CD (k3s), GitHub Actions + Ansible (Servarr VM)
- **Dependency Management:** Renovate
- **Monitoring:** Prometheus, Grafana, Alertmanager
- **Automation:** Ansible

## Infrastructure

- **Proxmox VE** — hypervisor running on Intel i5-14600K, 32GB RAM
- **UniFi UDM** — network management
- **HP EliteDesk 800s (x3)** — k3s cluster (i5-9500, 8GB RAM each)
  - k3s-master: <master-ip>
  - k3s-worker1: <worker1-ip>
  - k3s-worker2: <worker2-ip>

## Virtual Machines

| ID  | Type    | Name    | IP            | Description                       |
|-----|---------|---------|---------------|-----------------------------------|
| 100 | QEMU VM | HA      | <ha-ip> | Home Assistant, smart home        |
| 101 | LXC     | Samba   | <samba-ip> | File sharing, ZFS pool            |
| 102 | QEMU VM | Servarr | <servarr-ip> | Media stack (Jellyfin, arr stack) |

## Services

All services are exposed via Traefik with SSL termination using Cloudflare DNS challenge.

| Service        | URL                       | Backend                    |
|----------------|---------------------------|----------------------------|
| Dashboard      | dashboard.homelab.local     | k3s (Homepage)             |
| Grafana        | grafana.homelab.local       | k3s (monitoring ns)        |
| Miniflux       | rss.homelab.local           | k3s (miniflux ns)          |
| Memos          | memo.homelab.local          | k3s (memos ns)             |
| Mealie         | mealie.homelab.local        | k3s (mealie ns)            |
| IPTV Proxy     | iptv.homelab.local          | k3s (iptv ns)              |
| Jellyfin       | jellyfin.homelab.local      | <servarr-ip>:8096         |
| Seerr          | jellyseerr.homelab.local    | <servarr-ip>:5055         |
| Sonarr         | sonarr.homelab.local        | <servarr-ip>:8989         |
| Radarr         | radarr.homelab.local        | <servarr-ip>:7878         |
| Lidarr         | lidarr.homelab.local        | <servarr-ip>:8686         |
| Bazarr         | bazarr.homelab.local        | <servarr-ip>:6767         |
| Prowlarr       | prowlarr.homelab.local      | <servarr-ip>:9696         |
| NZBGet         | nzbget.homelab.local        | <servarr-ip>:6789         |
| Proxmox        | proxmox.homelab.local       | <proxmox-ip>:8006          |
| UniFi          | unifi.homelab.local         | <unifi-ip>:443            |
| Home Assistant | home.homelab.local          | Cloudflare Tunnel          |
| Traefik        | traefik.homelab.local       | k3s internal               |

## Repository Structure

```
homelab/
├── ansible/        # Ansible inventory and playbooks
├── homeassistant/  # Home Assistant configuration
├── jellyfin/       # Jellyfin compose (Servarr VM)
├── k3s/            # Kubernetes manifests and Flux GitOps config
├── proxmox/        # Proxmox notes and configuration
├── samba/          # Samba file server configuration
└── servarr/        # Arr stack Docker Compose (Servarr VM)
```

See [k3s/README.md](k3s/README.md) for detailed k3s architecture and GitOps workflow.

