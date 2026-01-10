# vLLM Setup Guide

## 1. Prerequisites

- **OS**: Linux (Windows not officially supported)
- **Python**: 3.10 - 3.13
- **GPU**: NVIDIA CUDA, AMD ROCm, or Intel XPU (CPU also supported)

---

## 2. Install vLLM

**Using pip (Simple):**

```bash
pip install vllm
```

**Using uv (Faster, Optional):**

```bash
# Install uv first
pip install uv

# Then install vLLM (--system for non-venv environments)
uv pip install vllm --torch-backend=auto --system
```

> ⚠️ For NVIDIA GPUs, the installer automatically selects the appropriate PyTorch/CUDA version.

---

## 3. Start the Server

```bash
# Start vLLM OpenAI-compatible server
vllm serve Qwen/Qwen2.5-1.5B-Instruct
```

> ⚠️ Keep this terminal open! Server runs at `http://localhost:8000` by default.

---

## 4. Verify in Terminal

Test the server using curl:

```bash
# List available models
curl http://localhost:8000/v1/models

# Chat completion request
curl http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "Qwen/Qwen2.5-1.5B-Instruct",
    "messages": [{"role": "user", "content": "Say hello!"}]
  }'
```

---

## 5. Verify in Jupyter Notebook

```python
%pip install openai
```

```python
from openai import OpenAI

# Connect to local vLLM server
client = OpenAI(base_url="http://localhost:8000/v1", api_key="dummy")

response = client.chat.completions.create(
    model="Qwen/Qwen2.5-1.5B-Instruct",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

---

## Troubleshooting

| Error | Solution |
| ------- | ---------- |
| CUDA out of memory | Use smaller model or add `--max-model-len 2048` |
| Model not found | Check HuggingFace model name or `huggingface-cli login` |
| Connection refused | Ensure `vllm serve` is running |
| PTX/CUDA toolchain error | Install `cuda-compat` package and update `LD_LIBRARY_PATH` |

---

## Useful Options

```bash
# Specify host and port
vllm serve Qwen/Qwen2.5-1.5B-Instruct --host 0.0.0.0 --port 8080

# Enable API key authentication
vllm serve Qwen/Qwen2.5-1.5B-Instruct --api-key your-secret-key
```

---

## Closing the Server

Press `Ctrl+C` in the terminal to stop the server.

---

**Official Docs**: [docs.vllm.ai](https://docs.vllm.ai)
