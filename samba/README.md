# Samba

Samba file server running as an LXC container on Proxmox.

## Setup

- Standard Ubuntu LXC on Proxmox
- Samba installed manually
- ZFS tank pool from Proxmox host mounted into the LXC

## Shares

| Share   | Path      | Access        | Notes                    |
|---------|-----------|---------------|--------------------------|
| data    | /data     | Authenticated | Media files, main storage |
| docker  | /docker   | Authenticated | Docker config files       |

## Network

- Only accessible from local network (192.168.0.0/17)
- Authentication required, no guest access
- Force user/group: jonas

## Restore

1. Create new LXC on Proxmox
2. Install Samba: `apt install samba`
3. Create user: `adduser jonas` and `smbpasswd -a jonas`
4. Mount ZFS tank pool from Proxmox host into LXC
5. Copy `smb.conf` from this repo to `/etc/samba/smb.conf`
6. Restart Samba: `systemctl restart smbd`

## Notes

- ZFS tank pool monitored via Proxmox VE integration in Home Assistant
- Future plan: migrate monitoring to Prometheus + Grafana
