# llama.cpp Setup Guide

## 1. Prerequisites

- **OS**: Windows, Linux, macOS
- **Python**: 3.8+
- **C compiler**: gcc/clang (Linux), Visual Studio/MinGW (Windows), Xcode (macOS)
- **GPU**: NVIDIA CUDA, AMD ROCm/HIP, Apple Metal, Vulkan, or CPU-only

---

## 2. Install llama-cpp-python

**CPU-only (Simple):**

```bash
pip install llama-cpp-python
```

**Pre-built CPU wheel (faster install):**

```bash
pip install llama-cpp-python \
  --extra-index-url https://abetlen.github.io/llama-cpp-python/whl/cpu
```

**With NVIDIA CUDA support:**

```bash
CMAKE_ARGS="-DGGML_CUDA=on" pip install llama-cpp-python

# Or use pre-built CUDA wheel (e.g., cu121)
pip install llama-cpp-python \
  --extra-index-url https://abetlen.github.io/llama-cpp-python/whl/cu121
```

**With Apple Metal support (macOS):**

```bash
CMAKE_ARGS="-DGGML_METAL=on" pip install llama-cpp-python

# Or use pre-built Metal wheel
pip install llama-cpp-python \
  --extra-index-url https://abetlen.github.io/llama-cpp-python/whl/metal
```

**With AMD ROCm/HIP support:**

```bash
CMAKE_ARGS="-DGGML_HIPBLAS=on" pip install llama-cpp-python
```

**With Vulkan support:**

```bash
CMAKE_ARGS="-DGGML_VULKAN=on" pip install llama-cpp-python
```

> ⚠️ If build fails, add `--verbose` to see the full cmake log.

---

## 3. Start the Server

**Install server extras:**

```bash
pip install 'llama-cpp-python[server]'
```

**Option A: Load model directly from Hugging Face (Recommended):**

```bash
python3 -m llama_cpp.server \
  --hf_model_repo_id Qwen/Qwen2.5-1.5B-Instruct-GGUF \
  --model '*q4_k_m.gguf' \
  --n_gpu_layers -1
```

**Option B: Load from local file:**

```bash
# Download model first
pip install huggingface_hub
hf download Qwen/Qwen2.5-1.5B-Instruct-GGUF qwen2.5-1.5b-instruct-q4_k_m.gguf --local-dir ./models

# Start server
python3 -m llama_cpp.server --model ./models/qwen2.5-1.5b-instruct-q4_k_m.gguf --n_gpu_layers -1
```

> ⚠️ Keep this terminal open! Server runs at `http://localhost:8000` by default.  
> Use `--n_gpu_layers -1` to offload all layers to GPU.

---

## 4. Verify in Terminal

```bash
# List available models
curl http://localhost:8000/v1/models

# Chat completion request
curl http://localhost:8000/v1/chat/completions \
  -H "Content-Type: application/json" \
  -d '{
    "model": "qwen2.5-1.5b-instruct-q4_k_m",
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

# Connect to local llama.cpp server
client = OpenAI(base_url="http://localhost:8000/v1", api_key="dummy")

response = client.chat.completions.create(
    model="qwen2.5-1.5b-instruct-q4_k_m",
    messages=[{"role": "user", "content": "Hello!"}]
)
print(response.choices[0].message.content)
```

---

## Troubleshooting

| Error | Solution |
| ------- | ---------- |
| Out of memory | Use smaller quant (Q2_K, Q3_K_S) or reduce `--n_gpu_layers` |
| Model format error | Ensure you're using GGUF format, not GGML or safetensors |
| Connection refused | Ensure `llama_cpp.server` is running |
| CUDA not detected | Reinstall with `CMAKE_ARGS="-DGGML_CUDA=on"` |
| Build fails | Add `--verbose` flag and check cmake output |

---

## Useful Options

```bash
# Specify host and port
python3 -m llama_cpp.server --model ./models/model.gguf --host 0.0.0.0 --port 8080

# Set context length
python3 -m llama_cpp.server --model ./models/model.gguf --n_ctx 4096

# Set chat format (check model card for correct format)
python3 -m llama_cpp.server --model ./models/model.gguf --chat_format chatml
```

---

## Closing the Server

Press `Ctrl+C` in the terminal to stop the server.

---

**Official Docs**: [llama-cpp-python.readthedocs.io](https://llama-cpp-python.readthedocs.io)  
**API Docs**: [http://localhost:8000/docs](http://localhost:8000/docs) (when server is running)
