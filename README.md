# Docker Compose Collection

Homelab Docker stacks managed by [Komodo](https://komo.do), running on Proxmox LXCs.

## Infrastructure

| LXC | Role |
|-----|------|
| Caddy LXC | Caddy reverse proxy + caddy-ui |
| App stacks LXC | Komodo Core + all app stacks |

## Stacks

| Stack | Host | Description |
|-------|------|-------------|
| caddy | Caddy LXC | Caddy reverse proxy + caddy-ui |
| channels-addons | App stacks LXC | MLBserver + FastChannels for Channels DVR |
| channels2mqtt | App stacks LXC | Channels DVR to Home Assistant via MQTT |
| container-backup-caddy | Caddy LXC | Backup spoke for Caddy LXC |
| container-backup-core | App stacks LXC | Backup hub for app stacks LXC |
| homepage | App stacks LXC | Homepage dashboard |
| it-tools | App stacks LXC | IT Tools developer utilities |
| radio | App stacks LXC | Icecast + Darkice internet radio |
| slash | App stacks LXC | Link shortener |
| transfersh | App stacks LXC | File transfer service |
| writefreely | App stacks LXC | Blog platform |

## Repository Structure

```bash
opt/stacks/<stack-name>/compose.yaml   # Docker Compose files
komodo/resources.toml                  # Komodo resource definitions
```

## Setup

### Prerequisites

- Proxmox with two Docker LXCs provisioned via [community-scripts](https://community-scripts.org)
- Komodo installed on the app stacks LXC via the Komodo addon script
- Komodo periphery agent installed on the Caddy LXC

### Restoring from Scratch

1. Provision two Docker LXCs using community-scripts
2. Install Komodo Core on the app stacks LXC:

   ```bash
   bash -c "$(curl -fsSL https://raw.githubusercontent.com/community-scripts/ProxmoxVE/main/tools/addon/komodo.sh)"
   ```

3. Install Komodo periphery on the Caddy LXC:

   ```bash
   curl -sSL https://raw.githubusercontent.com/moghtech/komodo/main/scripts/setup-periphery.py \
     | python3 - \
     --core-address="http://<komodo-ip>:9120" \
     --connect-as="caddy" \
     --onboarding-key="<onboarding-key>"
   ```

4. In Komodo, create a Resource Sync pointing to this repo at `komodo/resources.toml`
5. Execute the sync to restore all stacks
6. Restore persistent data from backups (see below)

### Restoring Persistent Data

Persistent data is backed up nightly to cloud storage via `container-backup`.

Containers with persistent data:

- `caddy` — Caddyfile, TLS data, logs
- `caddy-ui-backend` — caddy-ui config
- `homepage` — dashboard config
- `icecast` — icecast config
- `darkice` — darkice config
- `mlbserver` — MLB account data
- `slash` — link shortener database
- `transfersh` — uploaded files
- `writefreely` — blog database and keys

### Special Requirements

#### App stacks LXC

- Must be privileged for `/dev/snd` passthrough (required by `darkice`)
- Add to the LXC config in Proxmox:

  ```text
  lxc.cgroup2.devices.allow: c 116:* rwm
  lxc.mount.entry: /dev/snd dev/snd none bind,optional,create=dir
  ```

- Install rclone and the Docker rclone volume plugin for `container-backup-core`

#### Caddy LXC

- Enable FUSE in LXC features: `features: nesting=1,keyctl=1,fuse=1`
- Install rclone and the Docker rclone volume plugin for `container-backup-caddy`
- Port forward 80 and 443 to this LXC in your router

## Updating Stacks

Stacks are set to auto-update daily via Komodo's Global Auto Update procedure. To manually update a stack, deploy it from the Komodo UI.
