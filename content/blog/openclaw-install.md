---
title: "How to Install OpenClaw: The Step by Step Guide for 2026"
date: "2026-02-22"
author: "JD"
authorTitle: "Founder, Spark"
authorImage: "/images/jd-avatar.jpg"
authorLinkedIn: "https://linkedin.com/in/joshuaday"
category: "OpenClaw"
featured: false
excerpt: "A dead-simple, step-by-step tutorial to install OpenClaw on macOS, Linux, or Docker — from zero to chatting with your AI assistant in under 15 minutes."
coverImage: "https://images.ctfassets.net/placeholder"
hideTitle: false
---

You want to install OpenClaw. You've heard about the open-source AI assistant — one of the most talked-about AI agents (formerly [Clawdbot](https://github.com/openclaw/openclaw), briefly MoltBot) with 214,000+ stars on [GitHub](https://github.com/openclaw/openclaw), and you want to try it. This tutorial gets you from nothing to chatting with your own AI agent in under 15 minutes.

No fluff. No architecture lectures. Just the steps.

## Quick start: the fastest path

If you're on [macOS](https://www.apple.com/macos/) or [Linux](https://ubuntu.com/) and just want it running:

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

That's the [install.sh](https://openclaw.ai/install.sh) script from [openclaw.ai](https://openclaw.ai). It installs [Node.js](https://nodejs.org/) if you don't have it, installs OpenClaw globally via [npm](https://www.npmjs.com/), and launches the onboarding wizard.

On Windows (PowerShell):

```powershell
iwr -useb https://openclaw.ai/install.ps1 | iex
```

The onboarding wizard walks you through everything. But if you want to understand what's happening at each step — or if the script doesn't work for your setup — keep reading.

## What you need before you start

### Hardware

OpenClaw runs on basically anything with Node.js support:

- **Mac** (Intel or Apple Silicon) — the community favorite. Many people run it on a dedicated Mac Mini for always-on use.
- **Linux** — Ubuntu 22.04+ is the most common. Any distro with Node.js 22+ works.
- **Windows** — via WSL2 (Windows Subsystem for Linux). Native Windows support is limited.
- **VPS** — Hetzner, DigitalOcean, any cloud Linux box. Great for always-on without leaving a laptop open.
- **Virtual machine** — works fine if that's your preference.
- **Raspberry Pi** — technically works, but known stability issues with recent updates. Proceed with caution.

**Minimum:** 1 GB RAM for the gateway, 4 GB recommended.

### Dependencies

Just one real dependency: **Node.js 22 or higher**.

Check if you have it:
```bash
node --version
# Need v22.12.0 or higher
```

If you don't have Node.js, the install.sh script handles it for you. Or install it manually:

```bash
# macOS (Homebrew)
brew install node

# Ubuntu / Debian
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo bash -
sudo apt install -y nodejs

# Or use fnm/nvm for version management
curl -fsSL https://fnm.vercel.app/install | bash
fnm install 22
```

### An LLM API key

OpenClaw needs an AI model to think with. You need API keys from at least one provider:

| Provider | Get your key | Cost |
|----------|-------------|------|
| **[Anthropic](https://console.anthropic.com/)** (Claude) | console.anthropic.com | ~$5-20/mo typical |
| **[OpenAI](https://platform.openai.com/)** (GPT) | platform.openai.com | ~$5-20/mo typical |
| **Google Gemini** | aistudio.google.com | Free tier available |
| **OpenRouter** | openrouter.ai | Pay-per-use, many models |
| **Local (Llama via Ollama)** | ollama.com | Free (needs beefy GPU) |

**Recommendation for beginners:** Start with Anthropic (Claude). It gives the best results with OpenClaw. You can always add more providers later.

If you have an existing Claude Pro/Max or ChatGPT Plus subscription, you can use OAuth authentication instead of paying for API keys separately — the onboarding wizard supports both paths.

## Step by step: installing on macOS

### Step 1: Run the installer

Open Terminal and run:

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

This installs OpenClaw globally. When it finishes, it launches the onboarding wizard automatically.

### Step 2: Onboarding

The wizard asks you a few things:

1. **AI provider** — pick Anthropic, OpenAI, Google, or others. Paste your API keys when prompted.
2. **Messaging channel** — which chat app do you want to use? (You can skip this and use the built-in web chat.)
3. **Install as daemon?** — say yes. This registers OpenClaw with launchd so it runs in the background and survives reboots.

If you accidentally close the wizard, restart it:
```bash
openclaw onboard --install-daemon
```

### Step 3: Verify it's running

```bash
openclaw gateway status
```

You should see the gateway running on port 18789.

### Step 4: Start chatting

Open the dashboard:
```bash
openclaw dashboard
```

This opens `http://127.0.0.1:18789/` in your browser — the OpenClaw dashboard with built-in web chat. Type something. Your AI assistant is live.

**That's it.** Four steps. You have a working OpenClaw install with web chat. Everything else — Telegram, WhatsApp, skills, cron — is optional enhancement.

## Step by step: installing on Ubuntu / Linux

### Step 1: Install Node.js

```bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo bash -
sudo apt install -y nodejs
node --version  # confirm v22+
```

### Step 2: Install OpenClaw

```bash
npm install -g openclaw@latest
```

Or use the curl installer:
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

### Step 3: Run onboarding

```bash
openclaw onboard --install-daemon
```

On Linux, `--install-daemon` creates a systemd service. OpenClaw starts on boot, restarts on crash.

### Step 4: Verify and chat

```bash
openclaw gateway status
openclaw dashboard
```

Same as macOS — dashboard opens at `localhost:18789`, web chat is ready.

### For VPS users

If you're installing on a remote VPS (headless server), you can't open a browser directly. Access the dashboard via SSH tunnel:

```bash
# From your local machine:
ssh -N -L 18789:127.0.0.1:18789 user@your-vps-ip

# Then open http://localhost:18789 in your local browser
```

**Important:** Never expose port 18789 to the public internet. The OpenClaw gateway is an admin surface — anyone with access can control your AI agent, read your files, and execute commands. Use SSH tunnels, Tailscale, or a VPN.

## Step by step: installing with Docker

If you prefer containers:

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw
./docker-setup.sh
```

The Docker setup script builds the image, runs the onboarding wizard inside the container, and starts the gateway via Docker Compose.

Manual approach:
```bash
docker build -t openclaw:local -f Dockerfile .
docker compose run --rm openclaw-cli onboard
docker compose up -d openclaw-gateway
```

Docker is also great for running on a VPS without worrying about Node.js version conflicts or system dependencies.

## Connecting a messaging channel

Web chat works out of the box, but the real power of OpenClaw is texting your AI assistant from your phone. Here's how to connect the most popular channels:

### Telegram (easiest, recommended for beginners)

1. Open Telegram and search for **@BotFather**
2. Send `/newbot` and follow the prompts to create a bot
3. BotFather gives you a bot token — copy it
4. Add it to your OpenClaw config:
   ```bash
   openclaw config set channels.telegram.enabled true
   openclaw config set channels.telegram.token "YOUR_BOT_TOKEN"
   openclaw gateway restart
   ```
5. Open your new bot in Telegram and send a message
6. OpenClaw asks you to approve the pairing — run:
   ```bash
   openclaw pairing approve telegram <code>
   ```

Done. You can now text your AI agent from anywhere.

### WhatsApp

WhatsApp setup uses the Baileys library and requires scanning a QR code:

```bash
openclaw config set channels.whatsapp.enabled true
openclaw gateway restart
openclaw qr whatsapp  # displays QR code in terminal
```

Scan the QR code with WhatsApp on your phone (Settings → Linked Devices). Your phone number becomes the bot's number — messages to yourself become messages to your agent.

### Discord

1. Create a bot in the [Discord Developer Portal](https://discord.com/developers)
2. Copy the bot token
3. Configure:
   ```bash
   openclaw config set channels.discord.enabled true
   openclaw config set channels.discord.token "YOUR_BOT_TOKEN"
   openclaw gateway restart
   ```

### Slack

Uses the Bolt framework. Create a Slack app at [api.slack.com](https://api.slack.com/), configure OAuth scopes, install to your workspace, and add the credentials to your config file.

### iMessage

macOS only. Requires Full Disk Access permissions for the Messages database. The recommended path is BlueBubbles:

```bash
openclaw config set channels.imessage.enabled true
openclaw gateway restart
```

### Gmail integration

OpenClaw can read and send email through Gmail:

```bash
openclaw config set tools.gmail.enabled true
# Follow OAuth flow to authorize Gmail access
```

This gives your AI assistant the ability to triage your inbox, draft replies, and send messages — one of the most popular use cases.

## Understanding the key files

After installation, OpenClaw creates a home directory at `~/.openclaw/`. Here's what matters:

```
~/.openclaw/
├── openclaw.json          # Your config file (JSON5, supports comments)
├── credentials/           # API keys (stored with restricted permissions)
├── workspace/             # Your agent's brain
│   ├── AGENTS.md          # Operating instructions
│   ├── SOUL.md            # Personality and tone
│   ├── USER.md            # Info about you
│   ├── MEMORY.md          # Long-term memory
│   └── memory/            # Daily logs
└── sessions/              # Conversation history
```

The config file at `~/.openclaw/openclaw.json` is where all your settings live — channels, auth, model preferences, cron jobs. Edit it directly or use the CLI:

```bash
openclaw config get channels.telegram.enabled
openclaw config set agents.defaults.model anthropic/claude-opus-4-6
```

The workspace folder contains your agent's personality and memory — all stored as markdown files you can read and edit. Back this up to a private git repo.

## Essential CLI commands

These are the commands you'll actually use day-to-day:

```bash
# The big three
openclaw gateway status          # is it running?
openclaw doctor                  # diagnose problems (RUN AFTER EVERY UPDATE)
openclaw dashboard               # open the web UI

# Managing your agent
openclaw agent --message "..."   # send a message from terminal
openclaw logs --follow           # watch what your agent is doing
openclaw status                  # system overview

# Config
openclaw config get <key>        # read a setting
openclaw config set <key> <val>  # change a setting
openclaw configure               # interactive config editor

# Channels
openclaw channels status --probe # check all channels
openclaw gateway restart         # apply config changes

# Skills
openclaw plugins list            # see installed plugins
clawhub install <skill>          # install skills from ClawHub
```

## Install skills from ClawHub

[ClawHub](https://clawhub.com) is the plugin marketplace — 1,700+ community-built skills for everything from Apple Notes to Spotify to GitHub automation.

```bash
# Search for skills
clawhub search "gmail"

# Install a skill
clawhub install gmail-triage

# Update all installed skills
clawhub update --all
```

**Warning:** Review skills before you install them. Not all community skills are safe — some have been found to contain data exfiltration code. Stick to popular, well-reviewed skills when starting out.

## Troubleshooting

### "Gateway won't start"

The most common cause: invalid config file. OpenClaw uses strict validation — a typo or unknown key in `openclaw.json` will prevent startup.

```bash
openclaw doctor    # diagnoses and can auto-fix config issues
```

### "Channel not connecting"

```bash
openclaw channels status --probe    # shows what's connected/broken
openclaw gateway restart            # sometimes a restart fixes it
```

### "Agent not responding"

Check if the LLM provider is reachable:
```bash
openclaw models status    # shows which models are configured and working
```

Check your API keys haven't expired:
```bash
openclaw models auth status
```

### "Updated and everything broke"

This happens. Known issue. The fix:

```bash
openclaw doctor              # run this first, always
openclaw gateway restart     # restart after doctor finishes
```

If that doesn't work, check the [docs](https://docs.openclaw.ai) for version-specific migration notes, or the [GitHub issues](https://github.com/openclaw/openclaw/issues) for your specific error.

### Getting help

- **Docs:** [docs.openclaw.ai](https://docs.openclaw.ai) — comprehensive, searchable
- **Discord:** the #setup-help channel is active
- **GitHub Issues:** [github.com/openclaw/openclaw/issues](https://github.com/openclaw/openclaw/issues)
- **Reddit:** r/clawdbot

## Optimize your setup

Once you're running, a few tweaks to optimize your experience:

### Use model routing to save money

Don't use the most expensive LLM for everything. Set a default model for routine tasks and a premium model for complex work:

```bash
# Sonnet for everyday use (cheaper)
openclaw config set agents.defaults.model anthropic/claude-sonnet-4-6

# Override per-session when you need Opus
# (use /model command in chat)
```

### Set up your first automation

Try a simple morning briefing via cron:

```bash
openclaw cron add \
  --name "Morning briefing" \
  --cron "0 7 * * *" \
  --tz "America/Los_Angeles" \
  --message "Give me today's briefing: calendar, priority emails, weather" \
  --announce \
  --channel telegram
```

### Back up your workspace

Your agent's memory and personality live in `~/.openclaw/workspace/`. Version it:

```bash
cd ~/.openclaw/workspace
git init
git add -A
git commit -m "initial workspace backup"
git remote add origin git@github.com:you/openclaw-workspace.git
git push -u origin main
```

### Lock down permissions

If you're running on a shared machine or VPS:

```bash
openclaw security audit --deep --fix
```

This checks for common security issues and auto-fixes what it can. At minimum, make sure your gateway binds to localhost only and DM pairing is enabled.

---

That's the full install guide. The whole point of OpenClaw as an AI assistant is that it meets you where you are — in your chat app, on your schedule, with the workflows you define. The install is the easy part. The fun starts when you tell it what to do.

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

Go. 🦞

---

*Sources: [openclaw.ai](https://openclaw.ai), [docs.openclaw.ai](https://docs.openclaw.ai), [GitHub](https://github.com/openclaw/openclaw), [ClawHub](https://clawhub.com)*
