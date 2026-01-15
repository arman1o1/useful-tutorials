# llamafile Setup Guide

## 1. Prerequisites

- **OS**: Windows, Linux, macOS, FreeBSD, OpenBSD, NetBSD
- **GPU**: NVIDIA CUDA, AMD ROCm, Apple Metal, or CPU-only
- **No Python or dependencies required** — llamafile is a single self-contained executable

---

## 2. Download a llamafile

**Option A: Download from Hugging Face (Recommended):**

```bash
# Example: Download Qwen2.5-1.5B-Instruct (Q6_K quantization)
curl -LO https://huggingface.co/Bojun-Feng/Qwen2.5-1.5B-Instruct-GGUF-llamafile/resolve/main/qwen2.5-1.5b-instruct-q6_k.llamafile

# Or use wget
wget https://huggingface.co/Bojun-Feng/Qwen2.5-1.5B-Instruct-GGUF-llamafile/resolve/main/qwen2.5-1.5b-instruct-q6_k.llamafile
```

**Option B: Browse available llamafiles:**

Search for "llamafile" on [huggingface.co](https://huggingface.co/models?search=llamafile) or check [Mozilla's llamafile collection](https://huggingface.co/Mozilla).

---

## 3. Make Executable

**Linux/macOS/BSD:**

```bash
chmod +x qwen2.5-1.5b-instruct-q6_k.llamafile
```

**Windows:**

Rename the file to add `.exe` extension:

```text
qwen2.5-1.5b-instruct-q6_k.llamafile → qwen2.5-1.5b-instruct-q6_k.llamafile.exe
```

---

## 4. Start the Server

**Linux/macOS/BSD:**

```bash
./qwen2.5-1.5b-instruct-q6_k.llamafile --server --nobrowser
```

**Windows (PowerShell):**

```powershell
.\qwen2.5-1.5b-instruct-q6_k.llamafile.exe --server --nobrowser
```

> ⚠️ Keep this terminal open! Server runs at `http://localhost:8080` by default.  
> Remove `--nobrowser` to auto-open the built-in chat UI.

---

## 5. Verify in Terminal

```bash
# List available models
curl http://localhost:8080/v1/models

# Chat completion request
curl http://localhost:8080/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Qwen2.5-1.5B-Instruct",
    "messages": [{"role": "user", "content": "Say hello!"}]
  }'
```

---

## 6. Verify in Jupyter Notebook

```python
%pip install openai
```

```python
from openai import OpenAI

# Connect to local llamafile server (note: port 8080)
client = OpenAI(base_url="http://localhost:8080/v1", api_key="dummy")

response = client.chat.completions.create(
    model="Qwen2.5-1.5B-Instruct",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

---

## Troubleshooting

| Error | Solution |
| ------- | ---------- |
| Out of memory | Use smaller quant model (Q2_K, Q3_K_S) or reduce `--n-gpu-layers` |
| Connection refused | Ensure llamafile is running with `--server` flag |
| GPU not detected | Only NVIDIA driver needed; for AMD, install ROCm SDK v6.1 |
| Windows >4GB file | Use `--help` for workaround or split model weights |
| Permission denied | Run `chmod +x` (Linux/macOS) or add `.exe` (Windows) |

---

## Useful Options

```bash
# General syntax: ./model.llamafile [options]

# Specify host and port
./qwen2.5-1.5b-instruct-q6_k.llamafile --server --host 0.0.0.0 --port 8080

# Set context length
./qwen2.5-1.5b-instruct-q6_k.llamafile --server -c 4096

# GPU layers (offload all to GPU)
./qwen2.5-1.5b-instruct-q6_k.llamafile --server --n-gpu-layers 999

# CLI mode (no server, interactive chat)
./qwen2.5-1.5b-instruct-q6_k.llamafile

# Show all options
./qwen2.5-1.5b-instruct-q6_k.llamafile --help
```

---

## Closing the Server

Press `Ctrl+C` in the terminal to stop the server.

---

**Official Docs**: [github.com/Mozilla-Ocho/llamafile](https://github.com/Mozilla-Ocho/llamafile)  
**Model Hub**: [huggingface.co/Mozilla](https://huggingface.co/Mozilla)
