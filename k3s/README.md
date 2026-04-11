# k3s Cluster

Lightweight Kubernetes cluster running on 3x HP EliteDesk 800 G5, managed with Flux CD.

## Hardware

| Node        | IP            | CPU                  | RAM | Disk       |
|-------------|---------------|----------------------|-----|------------|
| k3s-master  | <master-ip> | Intel i5-9500 6-core | 8GB | 238GB NVMe |
| k3s-worker1 | <worker1-ip> | Intel i5-9500 6-core | 8GB | 238GB NVMe |
| k3s-worker2 | <worker2-ip> | Intel i5-9500 6-core | 8GB | 238GB NVMe |

**Total:** 18 cores, 24GB RAM, ~714GB storage

## OS & Installation

- Ubuntu Server 24.04 LTS
- SSH key authentication

### Master node
```bash
curl -sfL https://get.k3s.io | sh -
```

### Worker nodes
```bash
curl -sfL https://get.k3s.io | K3S_URL=https://<master-ip>:6443 K3S_TOKEN=<token> sh -
```

### Configure kubectl on local machine
```bash
scp jonas@<master-ip>:/home/jonas/k3s.yaml ~/.kube/config
sed -i 's/127.0.0.1/<master-ip>/' ~/.kube/config
```

## GitOps — Flux CD

All workloads are managed by [Flux CD](https://fluxcd.io/). Flux watches this repository and automatically applies changes within 1 minute of a push to main.

### How it works

```
GitHub (main branch)
└── k3s/flux/           ← Flux entry point
├── homepage.yaml   → watches k3s/homepage/
├── mealie.yaml     → watches k3s/mealie/
├── miniflux.yaml   → watches k3s/miniflux/
├── memos.yaml      → watches k3s/memos/
├── monitoring.yaml → watches k3s/monitoring/
└── external.yaml   → watches k3s/external/
```


### Adding a new app

1. Create `k3s/<appname>/` with Kubernetes manifests
2. Create `k3s/flux/<appname>.yaml` Kustomization pointing to that folder
3. Push to main — Flux reconciles automatically

### Bootstrap (cluster rebuild)

```bash
flux bootstrap github \
  --owner=yourusername \
  --repository=homelab \
  --branch=main \
  --path=k3s/flux \
  --personal
```

## Servarr VM Pipeline — GitHub Actions + Ansible

The Servarr VM (<servarr-ip>) runs Docker Compose stacks managed via Ansible. A self-hosted GitHub Actions runner on k3s-master triggers deployments on push.

- Renovate opens PRs for image version bumps
- Merging a PR triggers the Actions workflow
- Workflow runs `ansible/playbooks/servarr-deploy.yml`
- Playbook syncs compose files and recreates containers

## Ingress & SSL

Traefik handles ingress with Let's Encrypt SSL via Cloudflare DNS challenge. All IngressRoutes use `certResolver: cloudflare`.

Cloudflare API token stored as a Kubernetes secret:
```bash
kubectl create secret generic cloudflare-api-token \
  --from-literal=api-token=<token> \
  -n kube-system
```

## Folder Structure

```
k3s/
├── external/      # ExternalName ingresses (Proxmox, UniFi, Jellyfin, Servarr)
├── flux/          # Flux Kustomization resources (entry point for GitOps)
├── homepage/      # Homepage dashboard + ConfigMap config files
├── iptv/          # IPTV proxy + Gluetun VPN sidecar (secrets excluded from git)
├── mealie/        # Mealie recipe manager
├── memos/         # Memos notes app
├── miniflux/      # Miniflux RSS reader + PostgreSQL
└── monitoring/    # kube-prometheus-stack, exporters, PrometheusRules
```


## Recovery

In case of cluster rebuild:
1. Provision new nodes and install k3s
2. Re-create secrets (Cloudflare API token, Gluetun WireGuard key — stored in ProtonPass)
3. Bootstrap Flux — all workloads restore automatically
