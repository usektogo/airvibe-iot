# baseRapi — System Overview

> **AirVibe IoT Monitoring Network — Central Hub**
> Raspberry Pi 3 as reverse proxy, Grafana host, Tailscale gateway and web server.

---

## Hardware & OS

| Parameter | Value |
|-----------|-------|
| Device | Raspberry Pi 3 Model B v1.2 |
| OS | Raspberry Pi OS Lite (64-bit), no GUI |
| SD Card | 64 GB |
| Hostname | `baseRaPi.local` |
| Local IP | `<YOUR_LOCAL_IP>` |
| Tailscale IP | `<YOUR_TAILSCALE_IP>` |

---

## System Role

`baseRapi` is the **central hub** of the AirVibe IoT monitoring network. It serves as:

- **Reverse proxy** (Nginx) for the main domain and Grafana subdomain
- **Tailscale proxy** — forwards InfluxDB traffic from the Tailscale VPN to Synology NAS
- **Web server** — serves the AirVibe monitoring page
- **Grafana host** — dashboards accessible via HTTPS
- **SSH gateway** — central node for managing all remote Pi stations

---

## Network Architecture

```
                        🌍 INTERNET
                             │
                    Public IP (via Cloudflare Proxy)
                             │
                   +----------------------+
                   |   Router             |
                   |   Port fwd: 80, 443  |
                   |   → baseRapi         |
                   +----------------------+
                             │
                   +----------------------+
                   |      baseRapi        |
                   |   - Nginx            |
                   |   - Grafana :3000    |
                   |   - /var/www/html/   |
                   +----------------------+
                        │            │
              Tailscale VPN       Local LAN
                        │            │
             (remote Pi nodes)   +------------------+
                                 | Synology NAS     |
                                 | InfluxDB :8086   |
                                 +------------------+
```

### ⚠️ Known limitation: No Hairpin NAT on router
- From **outside** local network → public domain works ✅
- From **inside** local network → use local IP directly
- Cloudflare proxy resolved external access issues ✅

---

## InfluxDB (Synology NAS)

| Parameter | Value |
|-----------|-------|
| NAS local IP | `<NAS_IP>:8086` |
| Organisation | `airvibe` |

### Buckets

| Bucket | Description | Status |
|--------|-------------|--------|
| `aq_oms` | OMS Ljubljana – air quality (Enviro+) | ✅ active |
| `ms_oms` | OMS Ljubljana – weather (Enviro Weather/Pico) | ✅ active |
| `aq_lan` | Lancovo – air quality | ✅ active |
| `aq_off` | Office – air quality (indoor) | ✅ active |
| `Tlabs` | Lab upper + lower – temp/humidity (EE08) | ✅ active |
| `aq-po` | Postojna – air quality | ✅ active |
| `KG` | Kamna Gorica – air quality (kmetija + kamnolom) | ✅ active |
| `aq_mp` | Polhov Gradec (Martin Podlogar) – air quality | ✅ active |
| `system` | System health – universal, all stations | ✅ active |
| `aq_radovljica` | Radovljica – air quality | ❌ inactive |
| `ms_radovljica` | Radovljica – weather | ❌ inactive |

---

## Active Monitoring Stations (7 active)

| Name | Type | Bucket | Measurement | Coordinates |
|------|------|--------|-------------|-------------|
| INDOOR - Office | Indoor air quality | `aq_off` | `AQ_off` | 46.04502, 14.49052 |
| OUTDOOR - Ljubljana (OMS) | Outdoor air quality | `aq_oms` | `AQ_oms` | 46.05735, 14.50308 |
| OUTDOOR - Lancovo | Outdoor air quality | `aq_lan` | `AQ_lan` | 46.32914, 14.16608 |
| OUTDOOR - Kamna Gorica (kmetija) | Outdoor air quality | `KG` | `AQ_station` | 46.30815, 14.20319 |
| OUTDOOR - Polhov Gradec | Outdoor air quality | `aq_mp` | `AQ_station` | 46.06867, 14.31146 |
| OUTDOOR - Postojna | Outdoor air quality | `aq-po` | `AQ_po` | 45.77005, 14.21783 |
| T-labUp / T-labDown | Indoor temp/humidity | `Tlabs` | `env_reading` | — |

### ❌ Decommissioned stations

| Name | Reason |
|------|--------|
| Radovljica (`ws-rad`) | Station shut down, data preserved in InfluxDB |
| airtest (TSL2591) | Inactive, hostname unresolvable |

---

## Grafana

| Parameter | Value |
|-----------|-------|
| Local URL | `http://<LOCAL_IP>:3000` |
| Public URL | `https://grafana.<YOUR_DOMAIN>` |
| Config file | `/etc/grafana/grafana.ini` |

### Installation (working method)

Standard `apt install grafana` fails — use the official Grafana APT repo:

```bash
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/sources.list.d/grafana.list
sudo apt update
sudo apt install -y grafana
sudo systemctl enable --now grafana-server
```

### grafana.ini key settings

```ini
[server]
domain = grafana.<YOUR_DOMAIN>
root_url = %(protocol)s://grafana.<YOUR_DOMAIN>
http_addr = 0.0.0.0
serve_from_sub_path = false

[security]
allow_embedding = true

[auth.anonymous]
enabled = false
```

> 💡 To allow only specific public dashboards: `enabled = true` + `org_role = None`

### Folder Permissions

| Folder | Editor | Viewer |
|--------|--------|--------|
| AIRVIBE-Radovljica | View only | Disabled |
| AIRVIBE-OMS | Edit | Disabled |
| WEBPAGE | Disabled | View only |

---

## Tailscale Proxy for InfluxDB

Remote Pi nodes send data to baseRapi's Tailscale IP on port 8086.
baseRapi forwards via iptables NAT to the Synology NAS.

**Enable IP Forwarding** (`/etc/sysctl.conf`):
```
net.ipv4.ip_forward=1
```

```bash
sudo sysctl -p
```

**iptables NAT rules:**
```bash
sudo iptables -t nat -A PREROUTING -p tcp --dport 8086 -j DNAT --to-destination <NAS_IP>:8086
sudo iptables -t nat -A POSTROUTING -j MASQUERADE
```

**Persist rules:**
```bash
sudo apt install iptables-persistent
sudo netfilter-persistent save
sudo netfilter-persistent reload
```

**Tailscale setup:**
```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up --advertise-routes=192.168.0.0/24 --accept-routes
```

---

## Nginx — Reverse Proxy

| Domain / Path | Proxies to | Notes |
|--------------|-----------|-------|
| `<YOUR_DOMAIN>` | baseRapi :80 | Main web page |
| `www.<YOUR_DOMAIN>` | same | redirect |
| `grafana.<YOUR_DOMAIN>` | baseRapi :3000 | Grafana |
| `/api/v2/` | baseRapi :8086 | InfluxDB API for frontend JS |

### SSL Certificates (Let's Encrypt)

```bash
sudo apt install certbot python3-certbot-nginx -y

sudo certbot --nginx -d <YOUR_DOMAIN> -d www.<YOUR_DOMAIN> -d grafana.<YOUR_DOMAIN> \
  --email <YOUR_EMAIL> --agree-tos --non-interactive --redirect
```

Auto-renewal: `certbot.timer` + cron `0 3 * * * certbot renew --quiet`

```bash
sudo certbot renew --dry-run
sudo systemctl list-timers | grep certbot
```

**Always backup a working config:**
```bash
sudo cp /etc/nginx/sites-available/<YOUR_DOMAIN> ~/<YOUR_DOMAIN>_BACKUP_OK.conf
```

---

## systemd Services — Per Station

| Station | Service name | Log file | Interval |
|---------|-------------|----------|----------|
| aq-off | `socat-influx.service` | — | — |
| ws-oms | `aq_oms.service` | — | — |
| ws-lan | `log_to_influxdb.service` | `/home/pi/logs/aq_lancovo.log` | 15s |
| T-labUp | `ee08_logger.service` | `/home/pi/logs/labUp.log` | 60s |
| T-labDown | `ee08_logger.service` | `/home/pi/logs/labDown.log` | 60s |

### Remote status check from baseRapi
```bash
ssh pi@aq-off 'systemctl status socat-influx.service | grep Active'
ssh pi@ws-oms 'systemctl status aq_oms.service | grep Active'
ssh pi@ws-lan 'systemctl status log_to_influxdb.service | grep Active'
ssh pi@T-labUp 'systemctl status ee08_logger.service | grep Active'
ssh pi@T-labDown 'systemctl status ee08_logger.service | grep Active'
```

### Check all nodes at once
```bash
~/check_status_all.sh
```

---

## Watchdog

Config (`/etc/watchdog.conf`):
```
watchdog-device = /dev/watchdog
max-load-1 = 24
temperature-device = /sys/class/thermal/thermal_zone0/temp
max-temperature = 80000
```

```bash
sudo apt install watchdog
sudo systemctl enable watchdog
sudo systemctl start watchdog
```

**Fix corrupted installation:**
```bash
sudo dpkg --remove --force-remove-reinstreq watchdog
sudo apt clean && sudo apt update
sudo apt install watchdog
```

---

## SSH Key Setup

```bash
ssh-keygen -t ed25519 -C "your-label"
ssh-copy-id pi@<hostname>
ssh pi@<hostname> "echo OK"
```

---

## Active Cooling (Fan)

`/boot/firmware/config.txt`:
```
dtparam=fan_temp=55000
```

```bash
vcgencmd measure_temp
```

---

## Useful Commands

```bash
# baseRapi health
systemctl status grafana-server nginx tailscaled watchdog
df -h /
uptime

# Logs
sudo tail -f /var/log/nginx/access.log
sudo tail -f /var/log/nginx/error.log
```

---

## Open Issues

| Date | Issue | Status |
|------|-------|--------|
| 16.5.2025 | watchdog broken on ws-oms | ⚠️ Unresolved |
| 14.7.2025 | ws-oms went offline | ⚠️ Needs physical check |
| 19.3.2026 | Power outage → nginx/socat conflict + SSL 526 | ✅ Resolved |

---

## Related READMEs

- `README_grafana_influxdb.md`
- `README_sensor_stations.md`
- `README_nginx_config.md`
- `README_airvibe_webpage.md`
- `README_tailscale_proxy.md`

---

*Last updated: March 2026*
