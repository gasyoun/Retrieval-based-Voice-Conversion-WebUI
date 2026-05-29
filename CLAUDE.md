# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What This Repository Is

A fork of the [RVC-Project/Retrieval-based-Voice-Conversion-WebUI](https://github.com/RVC-Project/Retrieval-based-Voice-Conversion-WebUI) — an ML voice conversion tool based on VITS. This fork is maintained by the samskrtam.ru community and includes Russian-language voice samples in `golosa/` for use with the community's voice conversion experiments.

The upstream project is in Chinese; this fork adds Russian audio assets and applies community-specific patches. UI strings remain Chinese; code is English identifiers.

## Running the app

**Windows (recommended):**
```bat
go-web.bat              # Gradio web UI (training + inference)
go-realtime-gui.bat     # Real-time voice conversion GUI
go-web-dml.bat          # Same as go-web.bat but with Intel DML backend
go-realtime-gui-dml.bat # Real-time GUI with Intel DML backend
```

**Direct Python:**
```sh
python infer-web.py     # Web UI entry point
python gui_v1.py        # Legacy desktop GUI
```

**Docker:**
```sh
docker-compose up       # Build and run via Docker
```

## Requirements files

| File | Use when |
|---|---|
| `requirements.txt` | CUDA GPU (default) |
| `requirements-amd.txt` | AMD ROCm GPU |
| `requirements-dml.txt` | Intel GPU via DirectML |
| `requirements-ipex.txt` | Intel GPU via IPEX |
| `requirements-py311.txt` | Python 3.11 (pin overrides) |
| `requirements-win-for-realtime_vc_gui.txt` | Real-time GUI on Windows |
| `requirements-win-for-realtime_vc_gui-dml.txt` | Real-time GUI + Intel DML |

Install the base requirements with:
```sh
pip install -r requirements.txt
```

For the Conda/poetry environment, use `environment_dml.yaml` or `pyproject.toml` respectively.

## Key directories

| Path | Contents |
|---|---|
| `golosa/` | Russian-language audio samples added by this fork |
| `infer/` | Inference library (lib/, modules/) |
| `configs/` | Model configuration YAML files |
| `logs/` | Training logs and model checkpoints |
| `assets/` | Static assets for the Gradio UI |
| `tools/` | Utility scripts (pitch extraction, preprocessing) |
| `i18n/` | Internationalization locale files |

## Upstream relationship

This repo tracks the upstream `main` branch. The `sync_dev` CI workflow pulls upstream changes. When syncing:
- **Keep** `golosa/` additions — they are local to this fork.
- **Review** any changes to `requirements.txt` or `pyproject.toml` before accepting.
- The `.github/workflows/` files are partially from upstream and partially local.

## CI workflows

| Workflow | Trigger | Purpose |
|---|---|---|
| `docker.yml` | Push `v*` tag | Build and push Docker image to GHCR |
| `genlocale.yml` | Push / PR | Re-generate i18n locale files |
| `pull_format.yml` | Pull from upstream | Auto-format on sync |
| `push_format.yml` | Push to main | Format check on local pushes |
| `sync_dev.yml` | Schedule | Sync with upstream main |
| `unitest.yml` | Push / PR | Run unit tests |

## Windows / encoding notes

- FFmpeg binaries (`ffmpeg.exe`, `ffplay.exe`, `ffprobe.exe`) are bundled; no system install needed.
- Python 3.9–3.11 is supported; 3.12+ has known issues with `numba==0.56.4`.
- All audio files in `golosa/` use UTF-8 filenames with Russian Cyrillic; ensure your shell handles UTF-8.
- Use `sys.stdout.reconfigure(encoding='utf-8')` in any Python scripts that emit text.
