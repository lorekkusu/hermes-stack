# hermes-stack

Docker Compose stack for running Hindsight and Camofox.

Camofox uses `ghcr.io/lorekkusu/camofox:latest`, built from `jo-inc/camofox-browser` by GitHub Actions.

## Setup

1. Create your local environment file:

   ```sh
   cp .env.example .env
   ```

2. Edit `.env` and set your LLM and VNC settings:

   ```dotenv
   HINDSIGHT_LLM_PROVIDER=minimax
   HINDSIGHT_LLM_MODEL=MiniMax-M2.7
   HINDSIGHT_LLM_KEY=your-minimax-api-key

   CAMOFOX_ENABLE_VNC=1
   CAMOFOX_VNC_PASSWORD=change-me-vnc-password
   ```

3. Start the stack:

   ```sh
   docker compose up -d
   ```

4. Open Hindsight:

   ```text
   http://localhost:9999
   ```

## Ports

- `8888:8888` Hindsight API
- `9999:9999` Hindsight UI
- `9377:9377` Camofox API
- `5900:5900` Camofox VNC
- `6080:6080` Camofox noVNC UI

## Firewall

Do not expose these ports directly to the public internet. Keep them limited to localhost, a trusted private network, or an SSH/VPN tunnel.

- Restrict `8888`, `9999`, and `9377` because they expose agent/memory/browser APIs.
- Restrict `5900` and `6080` because they expose interactive VNC/noVNC browser access.
- If running this stack on a VPS, configure the host firewall or cloud security group before starting the services.
- For remote access, prefer SSH port forwarding, for example `ssh -L 9999:localhost:9999 -L 6080:localhost:6080 user@server`.

## Data

Hindsight data is stored in the Docker volume `hermes-stack_hindsight-database`.

Camofox profiles and cookies are stored in Docker volumes:

- `hermes-stack_camofox-profiles`
- `hermes-stack_camofox-cookies` mounted read-only

To stop the stack:

```sh
docker compose down
```

To stop the stack and remove persisted stack data:

```sh
docker compose down -v
```

## Camofox Image

The `Build Camofox` workflow builds `jo-inc/camofox-browser` with `make build` and pushes:

- `ghcr.io/lorekkusu/camofox:<version>` such as `ghcr.io/lorekkusu/camofox:135.0.1`
- `ghcr.io/lorekkusu/camofox:latest`

If the version tag already exists in GHCR, the workflow skips `make build` and refreshes `latest` from the existing version tag.
