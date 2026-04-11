# Home Assistant

Home Assistant running as a QEMU VM on Proxmox.

## Backup

- Automatic backups to Google Drive via built-in HA integration
- Restore: download latest backup from Google Drive and restore through HA UI

## External Access

- Exposed via Cloudflare Tunnel (country restricted to Finland)
- No port forwarding required
- Tunnel managed via **cloudflared** addon in Home Assistant
- Domain managed via Cloudflare DNS

## Restore

1. Create new QEMU VM on Proxmox
2. Install Home Assistant OS
3. Go through onboarding
4. Restore from Google Drive backup
5. Re-authenticate integrations if needed

## Integrations

- **Apple TV** — media player control
- **Google Cast** — casting support
- **Google Translate TTS** — text to speech
- **HACS** — community store for custom integrations
- **Meteorologisk institutt (Met.no)** — weather
- **Mobile App** — HA companion app (5 devices)
- **myUplink** — Nibe F470 heat pump monitoring
- **Nord Pool** — electricity spot prices
- **Panasonic Comfort Cloud** — AC control (currently failing to set up)
- **Philips Hue** — lighting (13 devices)
- **Proxmox VE** — Proxmox monitoring
- **Radio Browser** — internet radio
- **Shopping List** — built-in shopping list
- **SmartThings** — Samsung devices (3 devices)
- **Sun** — sun position/daylight
- **Team Tracker** — sports team tracking
- **Yale Smart Living** — smart locks (2 devices)

## HACS Components

### Integrations
- **myUplink** — Nibe heat pump (jaroschek)
- **Nibe Uplink** — Nibe heat pump legacy (elupus)
- **nordpool** — electricity spot prices (hellowlol)
- **Panasonic Comfort Cloud** — AC control (sockless-coding)
- **Proxmox VE** — Proxmox monitoring (dougiteixeira)
- **Team Tracker** — sports tracking (vasqued2)
- **Dreame Vacuum** — robot vacuum (tasshack)

### Frontend Plugins
- **Mushroom** — dashboard cards (piitaya)
- **Mushroom Dashboard Strategy** — auto dashboard (DigiLive)
- **apexcharts-card** — advanced charts (RomRider)
- **button-card** — custom buttons (custom-cards)
- **card-mod** — card styling (thomasloven)
- **mini-graph-card** — compact graphs (kalkih)
- **Team Tracker Card** — sports card (vasqued2)

## Notes

- No USB devices — all integrations are cloud or local IP based
- Hue Bridge communicates via local IP, no mDNS required
- Proxmox VE integration monitors the Proxmox host directly from HA
