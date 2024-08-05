# Docker Compose Collection

## Install Dockge

1. Create directories that store your stacks and stores Dockge's stack

```bash
mkdir -p /opt/stacks /opt/dockge
cd /opt/dockge
```

2. Download the compose.yaml

```bash
curl https://raw.githubusercontent.com/louislam/dockge/master/compose.yaml --output compose.yaml
```

3. Start the server

```bash
docker compose up -d
```

4. Download files from this repo to `/opt/stacks`

## Update Dockge

```bash
cd /opt/dockge
docker compose pull && docker compose up -d
```
