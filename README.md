# Voicebox Docker

Docker infrastructure to self-host [Voicebox](https://github.com/jamiepine/voicebox) — an AI voice/speech application.

The upstream source code is cloned from GitHub at build time; this repo only contains the Docker configuration.

## Architecture

| Service | Stack | Port |
|---------|-------|------|
| **Backend** | Python 3.12, NVIDIA CUDA 12.8.1, PyTorch, HuggingFace | `17493` |
| **Frontend** | Vite + React (built with Bun), served by nginx | `3000` |

## Prerequisites

- Docker and Docker Compose
- NVIDIA GPU with [NVIDIA Container Toolkit](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/install-guide.html) installed

## Quick Start

```bash
# Clone this repo
git clone https://github.com/YOUR_USER/voicebox-docker.git
cd voicebox-docker

# (Optional) Copy and edit environment variables
cp .env.example .env

# Build and start
docker compose up -d --build
```

The frontend will be available at `http://localhost:3000` and the backend API at `http://localhost:17493`.

## Configuration

### GPU Selection

By default, the backend uses GPU `0`. To change this:

```bash
# Use a specific GPU
CUDA_VISIBLE_DEVICES=1 docker compose up -d

# Use multiple GPUs
CUDA_VISIBLE_DEVICES=0,1 docker compose up -d
```

### Persistent Data

Two Docker volumes keep data across restarts:

| Volume | Mount Point | Purpose |
|--------|-------------|---------|
| `voicebox_data` | `/data` | Application data |
| `hf_cache` | `/root/.cache/huggingface` | Downloaded HuggingFace models |

## Common Commands

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# View logs
docker compose logs -f backend
docker compose logs -f frontend

# Rebuild a single service
docker compose build backend
docker compose build frontend
```

## License

This Docker configuration is provided as-is. Voicebox itself is maintained at [jamiepine/voicebox](https://github.com/jamiepine/voicebox).
