# OpenClaw + Local AI Models Setup Guide

A complete guide to installing OpenClaw with local AI models using Ollama — free, runs offline, no API costs.

---

## Overview

This guide covers:
1. Installing OpenClaw
2. Installing Ollama (local model runtime)
3. Downloading AI models (Gemma 2, Llama 3, etc.)
4. Configuring OpenClaw to use local models
5. Testing and verification

**Cost:** Software is free. You only pay for electricity and hardware.

---

## Prerequisites

| Requirement | Minimum | Recommended |
|-------------|---------|-------------|
| OS | macOS, Linux, Windows (WSL) | macOS, Linux |
| RAM | 8 GB | 16 GB |
| Storage | 10 GB free | 30 GB free |
| GPU | Optional | NVIDIA GPU (6+ GB VRAM) |

### Hardware Guide by Model Size

| Model Parameters | RAM | VRAM | Download Size |
|----------------|-----|------|--------------|
| 2-4B | 8 GB | 4 GB | 2-4 GB |
| 7-8B | 16 GB | 6 GB | 4-5 GB |
| 9B | 16 GB | 8 GB | 5-6 GB |
| 34B | 32 GB | 24 GB | 19-20 GB |
| 70B | 64 GB | 40 GB | 40 GB+ |

---

## Step 1: Install OpenClaw

### Basic Installation

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

### Non-Interactive Installation

```bash
curl -fsSL https://openclaw.ai/install.sh | bash -s -- --no-onboard
```

### Verify OpenClaw Installation

```bash
openclaw --version
```

If not found, add to PATH:

```bash
export PATH="$HOME/.local/bin:$PATH"
source ~/.bashrc  # or ~/.zshrc
```

---

## Step 2: Install Ollama

### macOS / Linux

```bash
curl -fsSL https://ollama.ai/install.sh | sh
```

### Enable Service (Linux)

```bash
sudo systemctl enable --now ollama
```

### Windows

Download from: https://ollama.com/download

### Verify Ollama is Running

```bash
curl http://localhost:11434/api/tags
```

Expected output: JSON with available models

---

## Step 3: Download Local Models

### Recommended Models

| Model | Command | Size | Best For |
|-------|---------|------|---------|
| **Gemma 2 2B** | `ollama pull gemma2` | 1.8 GB | Lightweight, fast |
| **Gemma 2 9B** | `ollama pull gemma2:9b` | 5.4 GB | Balanced |
| **Llama 3.2** | `ollama pull llama3.2` | 3.8 GB | General purpose |
| **Mistral** | `ollama pull mistral` | 4.1 GB | Fast, efficient |
| **CodeLlama** | `ollama pull codellama` | 3.8 GB | Code generation |
| **Phi 3** | `ollama pull phi3` | 2.3 GB | Lightweight |

### For Gemma 2 2B (User Requested)

```bash
# Pull the 2B parameter model
ollama pull gemma2
```

If you want specifically the 2B variant:

```bash
ollama pull gemma2:2b
```

### List Downloaded Models

```bash
ollama list
```

### Test Model Directly

```bash
ollama run gemma2 "Hello, how are you?"
```

---

## Step 4: Configure OpenClaw

### Find Your Config File

```bash
openclaw config edit
```

Or locate it manually:

```bash
openclaw doctor
```

The config is typically at: `~/.openclaw/openclaw.json`

### Add Ollama Provider Configuration

Edit your config file and add:

```json
{
  "models": {
    "providers": {
      "ollama": {
        "baseUrl": "http://localhost:11434"
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "ollama/gemma2"
      }
    }
  }
}
```

### Alternative: Using gateway.config.js

If using the newer config format (`gateway.config.js` in your project):

```javascript
// gateway.config.js
module.exports = {
  llms: {
    localOllama: {
      provider: "openai",
      url: "http://127.0.0.1:11434",
      models: {
        default: {
          name: "gemma2",
          temperature: 0.7,
          maxTokens: 2048
        },
        fast: {
          name: "mistral",
          temperature: 0.5
        }
      }
    }
  },
  defaultLlm: "localOllama.default"
};
```

### Important Notes

1. **Use native Ollama URL** — Use `http://localhost:11434`, NOT `http://localhost:11434/v1`
   - The `/v1` endpoint breaks tool calling
   - Models may output raw tool JSON as plain text

2. **API key field** — Ollama doesn't need an API key, but some configs require a placeholder:
   ```json
   "apiKey": "ollama-local"
   ```

---

## Step 5: Restart and Verify

### Restart OpenClaw

```bash
openclaw restart
```

### Check Model Status

```bash
openclaw models list
```

### Test with Chat

```bash
openclaw chat "Hello, what files are in this directory?"
```

### Monitor Logs

```bash
# Watch Ollama logs
ollama serve

# In another terminal, watch OpenClaw logs
openclaw logs
```

---

## Advanced Configuration

### Using Multiple Models

```json
{
  "models": {
    "providers": {
      "ollama": {
        "baseUrl": "http://localhost:11434"
      }
    }
  },
  "agents": {
    "defaults": {
      "model": {
        "primary": "ollama/gemma2",
        "fallbacks": [
          "ollama/llama3.2"
        ]
      }
    }
  }
}
```

### Hybrid Mode: Local + Cloud

Use local for simple tasks, cloud for complex ones:

```json
{
  "agents": {
    "defaults": {
      "model": {
        "primary": "ollama/gemma2",
        "fallbacks": ["anthropic/claude-sonnet-4-20250514"]
      }
    }
  }
}
```

### Custom Model Parameters

```json
{
  "llms": {
    "localOllama": {
      "provider": "openai",
      "url": "http://127.0.0.1:11434",
      "models": {
        "default": {
          "name": "gemma2",
          "temperature": 0.7,
          "maxTokens": 4096,
          "top_p": 0.9,
          "top_k": 40
        }
      }
    }
  }
}
```

---

## Performance Optimization

### Keep Model Loaded

By default, Ollama unloads models after a few minutes of inactivity. To keep it loaded:

```bash
export OLLAMA_KEEP_ALIVE=-1
```

Or configure in Ollama config at `~/.ollama/config.yaml`:

```yaml
keep_alive: -1
```

### GPU Offloading

Even partial GPU offloading dramatically improves speed. Ensure your GPU drivers are installed:

```bash
# Check NVIDIA drivers
nvidia-smi
```

### Choose the Right Model

| Task | Recommended Model |
|------|------------------|
| Simple chat / Q&A | phi3, gemma2:2b |
| General coding | codellama, llama3.2 |
| Complex reasoning | llama3.2:70b, gemma2:27b |
| Heartbeat tasks | 7-8B models |

### Monitor Resources

```bash
# Check RAM usage
free -h

# Check GPU usage
nvidia-smi

# Check disk space
df -h
```

---

## Troubleshooting

### "connection refused"

Ollama is not running.

```bash
# Start Ollama
ollama serve

# Or enable on boot (Linux)
sudo systemctl enable --now ollama
```

### "model not found"

Model name typo or not downloaded.

```bash
# Check exact name
ollama list

# Pull the model
ollama pull gemma2
```

### "out of memory"

Your system doesn't have enough RAM/GPU VRAM.

```bash
# Use a smaller model
ollama pull phi3

# Or use quantized version
ollama pull gemma2:2b-q4_0
```

### Slow Performance

1. Check GPU is being used:
   ```bash
   nvidia-smi
   ```

2. Keep model loaded:
   ```bash
   export OLLAMA_KEEP_ALIVE=-1
   ```

3. Use a smaller model for your hardware

### Tool Calling Not Working

**Solution:** Use native Ollama URL (`http://localhost:11434`), NOT `/v1` endpoint.

---

## Uninstalling

### Remove Ollama

```bash
# Stop service
sudo systemctl stop ollama
sudo systemctl disable ollama

# Remove files
rm -rf ~/.ollama
rm -rf /usr/local/bin/ollama
rm -rf /opt/ollama
```

### Remove OpenClaw

```bash
npm uninstall -g openclaw
rm -rf ~/.openclaw
```

---

## Quick Reference Commands

```bash
# Install OpenClaw
curl -fsSL https://openclaw.ai/install.sh | bash

# Install Ollama
curl -fsSL https://ollama.ai/install.sh | sh

# Pull Gemma 2 (2B)
ollama pull gemma2

# List models
ollama list

# Start Ollama
ollama serve

# Test model
ollama run gemma2 "Hello"

# OpenClaw config
openclaw config edit

# Restart OpenClaw
openclaw restart

# Check status
openclaw models list
```

---

## Resources

- OpenClaw: https://openclaw.ai
- Ollama: https://ollama.ai
- Ollama Models: https://ollama.ai/library
- OpenClaw Docs: https://docs.openclaw.ai

---

**Last Updated:** April 2026

### Thank you 
Contact if you need help and support: 
[EFXTV](https://t.me/efxtv)

[![Buy Me a Coffee](https://img.shields.io/badge/Buy%20Me%20a%20Coffee-ffdd00?style=for-the-badge&logo=buy-me-a-coffee&logoColor=black)](https://buymeacoffee.com/efxtv)
