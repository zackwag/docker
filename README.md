# Docker Compose Collection

## Installing Dockge

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

## Restoring Stacks

1. Download files from this repo to `/opt/stacks`

## Restoring Persistent Data

1. Download backups

## Updating Dockge

1. Run the following command

    ```bash
    cd /opt/dockge
    docker compose pull && docker compose up -d
    ```
