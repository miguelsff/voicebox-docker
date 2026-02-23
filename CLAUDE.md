# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Overview

This repository contains Docker infrastructure to self-host [Voicebox](https://github.com/jamiepine/voicebox) — an AI voice/speech application. The upstream source code is cloned from GitHub at build time; this repo only contains the Docker configuration.

**Architecture:**
- `backend`: Python 3.12 API server (port 17493), GPU-accelerated via NVIDIA CUDA 12.8.1, uses PyTorch and HuggingFace models
- `frontend`: Vite/React SPA built with Bun, served by nginx on port 3000

## Common Commands

```bash
# Start all services
docker compose up -d

# Build and start (after config changes)
docker compose up -d --build

# Stop services
docker compose down

# View logs
docker compose logs -f backend
docker compose logs -f frontend

# Use a specific GPU
CUDA_VISIBLE_DEVICES=1 docker compose up -d

# Rebuild a single service
docker compose build backend
docker compose build frontend
```

## Key Configuration Details

- Backend data is persisted in Docker volume `voicebox_data` (mounted at `/data`)
- HuggingFace model cache is persisted in Docker volume `hf_cache` (at `/root/.cache/huggingface`)
- GPU access requires the NVIDIA Container Toolkit on the host; `CUDA_VISIBLE_DEVICES` defaults to `0`
- The frontend build skips TypeScript type-checking (`tsc`) because upstream Tauri-specific code causes errors that don't affect the web build
- nginx serves the SPA with SPA fallback routing and aggressive caching for static assets
