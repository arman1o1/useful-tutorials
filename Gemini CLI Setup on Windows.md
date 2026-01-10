# Gemini CLI Setup on Windows

A guide to install and configure Gemini CLI with secure API key management.

---

## Prerequisites

- **Node.js** (v18 or later) - [Download](https://nodejs.org/)
- **Google AI Studio** account - [Get API Key](https://aistudio.google.com/apikey)

---

## Installation

```powershell
npm install -g @google/gemini-cli
```

Verify installation:

```powershell
gemini --version
```

---

## First Run

When you run `gemini` for the first time, you'll be prompted to choose an authentication method:

```
? How would you like to authenticate?
> Login with Google
  Use an API Key
```

Select your preferred method and follow the prompts.

---

## Authentication Methods

### Option 1: Google OAuth Login (Most Secure)

The simplest and most secure method - no API key management required:

```powershell
gemini
```

1. Select **"Login with Google"** when prompted
2. Browser opens automatically
3. Sign in with your Google account
4. Authorization completes automatically

> ✅ **Benefits:** No keys to manage, automatic token refresh, tied to your Google account.

---

### Option 2: Environment Variable (Recommended for API Keys)

**Permanent Setup:**

```powershell
# Set user environment variable (persists across sessions)
[Environment]::SetEnvironmentVariable("GEMINI_API_KEY", "your-api-key-here", "User")
```

**Temporary (Current Session Only):**

```powershell
$env:GEMINI_API_KEY = "your-api-key-here"
```

> 💡 **Tip:** Restart your terminal after setting a permanent environment variable.

---

### Option 3: Windows Credential Manager

Store API key securely in Windows Credential Manager:

```powershell
# Install credential helper
Install-Module -Name CredentialManager -Scope CurrentUser

# Store credential
New-StoredCredential -Target "GEMINI_API_KEY" -UserName "gemini" -Password "your-api-key-here" -Type Generic -Persist LocalMachine
```

Retrieve in scripts:

```powershell
$cred = Get-StoredCredential -Target "GEMINI_API_KEY"
$env:GEMINI_API_KEY = $cred.GetNetworkCredential().Password
gemini
```

---

## Running Gemini CLI

```powershell
# Start interactive mode
gemini

# Single query
gemini -p "Your prompt here"
```

---

## Best Practices

| Practice | Description |
|----------|-------------|
| **Prefer OAuth** | Use Google login when possible - no keys to leak |
| **Never hardcode keys** | Use environment variables or credential managers |
| **Rotate keys regularly** | Generate new keys periodically in AI Studio |
| **Use .gitignore** | Exclude any files containing credentials |

---

## Troubleshooting

| Issue | Solution |
|-------|----------|
| `GEMINI_API_KEY not found` | Restart terminal after setting env variable |
| `Invalid API key` | Verify key at [AI Studio](https://aistudio.google.com/apikey) |
| `npm install fails` | Run PowerShell as Administrator |
| OAuth login fails | Check browser pop-up blocker settings |

---

## Useful Commands

```powershell
# Check if API key is set
echo $env:GEMINI_API_KEY

# View Gemini CLI help
gemini --help

# Update Gemini CLI
npm update -g @google/gemini-cli
```

---

## Resources

- [Gemini CLI Documentation](https://github.com/google-gemini/gemini-cli)
- [Google AI Studio](https://aistudio.google.com/)
- [API Key Best Practices](https://cloud.google.com/docs/authentication/api-keys)
