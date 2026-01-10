# Gemini CLI Setup on Cloud (JupyterLab Terminal)

A guide to install and configure Gemini CLI in JupyterLab terminal on cloud platforms.

---

## Prerequisites: Install Node.js

If `npm` is not available, install Node.js first:

```bash
# Install Node.js via NodeSource (recommended)
curl -fsSL https://deb.nodesource.com/setup_20.x | bash -
apt-get install -y nodejs

# Verify installation
node --version
npm --version
```

---

## Installation

Open the JupyterLab terminal and run:

```bash
npm install -g @google/gemini-cli@latest
```

Verify installation:

```bash
gemini --version
```

> ⚠️ **Note:** Installation resets when the cloud session ends. Re-run on new sessions.

---

## Secure API Key Setup

### Option 1: Export in Terminal (Session Only)

```bash
export GEMINI_API_KEY="your-api-key-here"
gemini
```

---

### Option 2: Shell Profile (Persistent)

Add to `~/.bashrc` or `~/.zshrc`:

```bash
echo 'export GEMINI_API_KEY="your-api-key-here"' >> ~/.bashrc
source ~/.bashrc
```

---

### Option 3: .env File

```bash
# Create .env file
echo "GEMINI_API_KEY=your-api-key-here" > ~/.gemini_env

# Load before running
source ~/.gemini_env
gemini
```

---

## Running Gemini CLI

```bash
# Start interactive mode
gemini

# Single query
gemini -p "Your prompt here"
```

---

## Best Practices

| Practice | Description |
| ---------- | ------------- |
| **Never hardcode in notebooks** | Keep keys in terminal env only |
| **Use .gitignore** | Exclude credential files |
| **Clear history** | Run `history -c` after pasting keys |

---

## Troubleshooting

| Issue | Solution |
| ------- | ---------- |
| `gemini: command not found` | Reinstall or add npm bin to PATH |
| `GEMINI_API_KEY not found` | Re-export the variable |
| `Permission denied` | Use `sudo npm install -g @google/gemini-cli` |

---

## Cleanup: Remove API Key

After your work session, remove the API key from memory:

```bash
# Remove from current session
unset GEMINI_API_KEY

# Clear terminal history (removes any pasted keys)
history -c

# Verify removal
echo $GEMINI_API_KEY
# Should output nothing
```

> 💡 **Tip:** Cloud sessions auto-reset, but it's good practice to clean up immediately after work.

---

## Resources

- [Gemini CLI Documentation](https://github.com/google-gemini/gemini-cli)
- [Google AI Studio - Get API Key](https://aistudio.google.com/apikey)
