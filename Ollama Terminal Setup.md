# Ollama Setup Guide

## 1. Install Ollama

Download from: **[ollama.ai](https://ollama.ai)**

Or via command line:

- **Windows**: `winget install Ollama.Ollama`
- **Mac**: `brew install ollama`
- **Linux**: `curl -fsSL https://ollama.com/install.sh | sh`

---

```bash
# Start the server
ollama serve
```

> ⚠️ Keep this terminal open! The server must be running.
---

## 2. Terminal Setup

Open a new **system terminal** (PowerShell/CMD/Bash) and run:

```bash
# chech ollama version
ollama --version

# Pull a model (choose one)
ollama pull llama3.2
# Or pull a smaller model if you have limited resources
ollama pull llama3.2:1b
# OR other model:
ollama pull phi3

#Verify Downloaded Models
ollama list
```

## 3. Verify in Terminal

Test the model(should be downloaded first):

```bash
ollama run llama3.2 "Say hello!"
```

You should see a response from the model.

---

## 4. Verify in Jupyter Notebook

```python
%pip install langchain_ollama
```

```python
from langchain_ollama import ChatOllama

# Use the same model you pulled
llm = ChatOllama(model="llama3.2")
response = llm.invoke("Hello!")
print(response.content)
```

---

## Troubleshooting

| Error | Solution |
| ------- | ---------- |
| Model not found | Run `ollama pull <model>` first |
| Connection refused | Run `ollama serve` in terminal |
| Check available models | Run `ollama list` |

---

## Closing the server

Don't forget to close the server after use via CTR+C.

---
