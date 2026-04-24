# AGENTS.md

## Repo Shape

- This is an infra-only Docker Compose repo; there are no package manifests, lockfiles, app sources, or test suites.
- Highest-signal files are `compose.yaml`, `.env.example`, `README.md`, and `.github/workflows/build-camofox.yml`.

## Compose And Env

- Keep default values in `.env.example`, not in `compose.yaml`.
- Never commit `.env`; update `.env.example` when changing env shape or documented defaults.
- Hindsight `.env` vars are scoped as `HINDSIGHT_LLM_PROVIDER`, `HINDSIGHT_LLM_MODEL`, and `HINDSIGHT_LLM_KEY`, then mapped to container vars `HINDSIGHT_API_LLM_*` in `compose.yaml`.
- Camofox env in `compose.yaml` should stay VNC-only: `CAMOFOX_ENABLE_VNC` maps to `ENABLE_VNC`, and `CAMOFOX_VNC_PASSWORD` maps to `VNC_PASSWORD`.
- Do not use `network_mode: host`; expose ports explicitly: `8888` Hindsight API, `9999` Hindsight UI, `9377` Camofox API, `5900` VNC, `6080` noVNC UI.
- Use Docker named volumes, not host bind mounts: `hindsight-database`, `camofox-profiles`, and read-only `camofox-cookies`.

## Camofox Image

- Runtime Camofox image is `ghcr.io/lorekkusu/camofox:latest`; do not revert to local `camofox-browser:*` tags in compose.
- `Build Camofox` checks out external `jo-inc/camofox-browser`, reads upstream Makefile `VERSION` and `RELEASE` when workflow inputs are blank, then runs `make build`.
- The workflow pushes `ghcr.io/<owner>/camofox:<version>` and `ghcr.io/<owner>/camofox:latest`.
- If the `<version>` tag already exists in GHCR, the workflow skips `make build` and refreshes `latest` from the existing version tag.
- Image tags intentionally omit arch; default `x86_64` avoids tag collisions unless the workflow is changed for multi-arch publishing.
- GitHub Actions use major tags here, such as `actions/checkout@v6` and `docker/login-action@v4`.

## Verification

- Validate compose wiring with non-empty secret placeholders: `HINDSIGHT_LLM_PROVIDER=minimax HINDSIGHT_LLM_MODEL=MiniMax-M2.7 HINDSIGHT_LLM_KEY=test-key CAMOFOX_ENABLE_VNC=1 CAMOFOX_VNC_PASSWORD=test-password docker compose config`.
- Validate workflow semantics with `actionlint .github/workflows/build-camofox.yml` when `actionlint` is installed.
- Local workflow fallback check: `ruby -e 'require "yaml"; YAML.load_file(".github/workflows/build-camofox.yml")'`.
- Always run `git diff --check` after edits.
