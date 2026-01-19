# Use Claude Code with Ollama Models

This guide shows how to run Claude Code locally using Ollama models.

## Prerequisites

- **Operating System**: Windows 10+ (with WSL), macOS 10.15+, or Ubuntu 20.04+/Debian 10+
- **Hardware**: Minimum 4GB RAM (more recommended for larger models)
- **Software**: Node.js 18+
- **Ollama Version**: v0.14.0 or later (required for Anthropic Messages API compatibility)

---

## Step 1: Install Ollama

### Windows

Download the installer from [ollama.com](https://ollama.com/download) and run the `.exe` file.

### macOS

```bash
brew install --cask ollama
```

Or download from [ollama.com](https://ollama.com/download).

### Linux

```bash
curl -fsSL https://ollama.com/install.sh | sh
```

> [!NOTE]
> If you are in a Jupyter environment or minimal Linux setup, you may need `zstd` to decompress the installer. Run the following to install it:
>
> ```bash
> apt-get update && apt-get install -y zstd
> ```

### Verify Installation

```bash
ollama -v
```

> [!TIP]
> If you are on Linux/Jupyter and the server doesn't start automatically, run:
>
> ```bash
> ollama serve &
> ```

---

## Step 2: Download a Model

Pull a coding-optimized model with Ollama. Recommended models:

| Model | Command | Size | Notes |
| :--- | :--- | :--- | :--- |
| Qwen3 Coder (30B) | `ollama pull qwen3-coder:30b` | ~19GB | Latest agentic code model |
| GPT-OSS (20B) | `ollama pull gpt-oss:20b` | ~13GB | Efficient coding specialist |

Example:

```bash
ollama pull gpt-oss:20b
```

> [!TIP]
> For optimal Claude Code performance, use models with at least **64k context length**.

---

## Step 3: Start Ollama Server

Ensure the Ollama server is running. In a detached terminal or background:

```bash
ollama serve
```

**For Jupyter/Cloud environments:**
Run in a separate terminal or use background execution:

```bash
ollama serve > ollama.log 2>&1 &
```

The server runs on `http://localhost:11434` by default.

---

## Step 4: Install Claude Code

### macOS / Linux / WSL

```bash
curl -fsSL https://claude.ai/install.sh | bash
```

> [!IMPORTANT]
> If you see a warning that `~/.local/bin` is not in your PATH, run:
>
> ```bash
> echo 'export PATH="$HOME/.local/bin:$PATH"' >> ~/.bashrc && source ~/.bashrc
> ```

### Windows (PowerShell)

```powershell
irm https://claude.ai/install.ps1 | iex
```

### Windows (CMD)

```cmd
curl -fsSL https://claude.ai/install.bat | cmd
```

---

## Step 5: Configure Environment Variables

Set these environment variables to point Claude Code to your local Ollama instance:

### Linux / macOS / WSL

Add to your `~/.bashrc`, `~/.zshrc`, or equivalent:

```bash
export ANTHROPIC_AUTH_TOKEN="ollama"
export ANTHROPIC_BASE_URL="http://localhost:11434"
export ANTHROPIC_MODEL="gpt-oss:20b"
export CLAUDE_CODE_USE_ANTHROPIC="1"
```

Then reload:

```bash
source ~/.bashrc  # or ~/.zshrc
```

### Windows (PowerShell - Session)

```powershell
$env:ANTHROPIC_AUTH_TOKEN = "ollama"
$env:ANTHROPIC_BASE_URL = "http://localhost:11434"
$env:ANTHROPIC_MODEL = "gpt-oss:20b"
$env:CLAUDE_CODE_USE_ANTHROPIC = "1"
```

### Windows (Permanent - System Environment Variables)

1. Open **System Properties** → **Advanced** → **Environment Variables**
2. Add the following user variables:
   - `ANTHROPIC_AUTH_TOKEN` = `ollama`
   - `ANTHROPIC_BASE_URL` = `http://localhost:11434`
   - `ANTHROPIC_MODEL` = `gpt-oss:20b`
   - `CLAUDE_CODE_USE_ANTHROPIC` = `1`

---

## Step 6: Run Claude Code

Navigate to your project directory and start Claude Code:

```bash
cd your-project-directory
```

You can specify the model in two ways:

### Option 1: Using the `--model` flag (Overrides environment variables)

```bash
claude --model gpt-oss:20b
```

### Option 2: Using the `ANTHROPIC_MODEL` environment variable (If configured in Step 5)

```bash
claude
```

---

## Environment Variables Reference

| Variable | Value | Description |
| :--- | :--- | :--- |
| `ANTHROPIC_AUTH_TOKEN` | `ollama` | Required by Claude Code, ignored by Ollama |
| `ANTHROPIC_BASE_URL` | `http://localhost:11434` | Points to local Ollama server |
| `ANTHROPIC_MODEL` | `<model-name>` | Model to use (e.g., `gpt-oss:20b`) |
| `CLAUDE_CODE_USE_ANTHROPIC` | `1` | Forces use of Anthropic-compatible API |

---

## Troubleshooting

### Connection Refused

Ensure Ollama server is running:

```bash
ollama serve
```

### Model Not Found

Verify the model is downloaded:

```bash
ollama list
```

### Slow Performance

- Use a smaller model variant if resources are limited
- Ensure GPU acceleration is enabled in Ollama
- Check available VRAM with `nvidia-smi` (NVIDIA) or equivalent

### CORS Issues

If you encounter cross-origin issues, set:

```bash
export OLLAMA_ORIGINS="*"
```

---

## Recommended Hardware

| Model Size | Minimum VRAM | Recommended |
| :--- | :--- | :--- |
| 8B params | 8GB | 12GB |
| 30B params | 24GB | 32GB |
| 70B+ params | 48GB+ | 80GB+ |

---

## Stop Ollama Server

To completely stop the Ollama server and free up resources:

**Linux / macOS:**

```bash
pkill ollama
```

**Windows:**

1. Look for the **Ollama icon** in the system tray (bottom right).
2. Right-click and select **Quit Ollama**.
3. Alternatively, kill the process via PowerShell:

```powershell
Stop-Process -Name "ollama*" -Force
```

---

## References

- [Ollama Official Site](https://ollama.com)
- [Ollama Anthropic API Compatibility](https://ollama.com/blog/claude)
