# Homelab: Home Assistant with a local LLM on a Raspberry Pi 5

Home Assistant, Ollama and a fully local voice pipeline on one Raspberry Pi 5 (16 GB RAM, 1 TB NVMe), run as rootful Podman containers managed by systemd Quadlet units.

| Service | Role | Port |
|---|---|---|
| Home Assistant | Automation hub | 8123 |
| Ollama (`qwen2.5:3b`) | Conversation agent with tool calling | 127.0.0.1:11434 |
| Wyoming Whisper (`base-int8`) | Speech-to-text | 127.0.0.1:10300 |
| Wyoming Piper | Text-to-speech | 127.0.0.1:10200 |
| Open WebUI | Chat UI for Ollama | 3000 |

## Layout

- `configuration.yaml`, `packages/`, `dashboards/`, `themes/`, `blueprints/`: the Home Assistant config, mounted at `/config`.
- `containers/`: copies of the Quadlet units from `/etc/containers/systemd/`. Refresh with `sudo cp /etc/containers/systemd/*.container containers/`.

## Deploying a container change

    sudo cp containers/*.container /etc/containers/systemd/
    sudo systemctl daemon-reload && sudo systemctl restart <name>

Upgrades are a one-line change to the pinned `Image=` in the unit file.

Secrets, `.storage/`, the database and third-party `custom_components/` are deliberately not tracked; see `.gitignore`.
