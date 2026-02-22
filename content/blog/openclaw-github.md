---
title: "OpenClaw on GitHub: The Complete Guide to the Codebase, Updates, and Deployment"
date: "2026-02-22"
author: "JD"
authorTitle: "Founder, Spark"
authorImage: "/images/jd-avatar.jpg"
authorLinkedIn: "https://linkedin.com/in/joshuaday"
category: "OpenClaw"
featured: false
excerpt: "Everything you need to know about the OpenClaw GitHub repository — the codebase architecture, how to update without breaking things, production vs beta, Docker deployment, multi-agent routing, and how to contribute."
coverImage: "https://images.ctfassets.net/placeholder"
hideTitle: false
---

[OpenClaw](https://openclaw.ai) has 214,000+ stars on [GitHub](https://github.com/openclaw/openclaw). That makes it one of the most popular open-source projects on the platform — period. But starring a repo and actually understanding it are different things.

If you want to use OpenClaw seriously — update it without breaking your setup, deploy it on a VPS, understand the codebase, run multi-agent routing, or contribute back — this is the guide. Written for beginners who just cloned the repo and power users who want to understand what's under the hood.

## The repo at a glance

**URL:** [github.com/openclaw/openclaw](https://github.com/openclaw/openclaw)

OpenClaw is a TypeScript and Swift project created by [Peter Steinberger](https://steipete.me) ([@steipete](https://x.com/steipete)), the Austrian developer behind PSPDFKit. It started as **Clawd** — a personal AI assistant he built in an hour by wiring WhatsApp to Claude's CLI. It went viral as **Clawdbot**, got a trademark complaint from [Anthropic](https://www.anthropic.com/), became **MoltBot** for three days, and finally settled on **OpenClaw**. The mascot is a space lobster named **Molty** 🦞. The motto is "EXFOLIATE! EXFOLIATE!"

| Fact | Detail |
|------|--------|
| Stars | 214,000+ |
| Forks | 41,400+ |
| License | MIT |
| Language | TypeScript (Node.js ≥22), Swift (native apps) |
| Package manager | pnpm (npm and bun also work) |
| Versioning | CalVer — `YYYY.M.D` (e.g., `2026.2.22`) |
| Creator | Peter Steinberger, now at [OpenAI](https://openai.com/) |
| Maintainers | 12+ named contributors |
| Org repos | 19 total under [github.com/openclaw](https://github.com/openclaw) |

Steinberger joined OpenAI in February 2026 and moved the project to an independent open-source foundation. Development continues at full speed.

## Understanding the codebase

Here's what you're looking at when you open the repo:

```
openclaw/
├── openclaw.mjs           # CLI entry point
├── package.json            # CalVer version, deps, scripts
├── src/                    # Core TypeScript source
│   ├── gateway/            # WebSocket server, protocol, RPC methods
│   ├── agents/             # Agent runtime (Pi agent), system prompts
│   ├── config/             # Zod schema, validation, hot-reload
│   ├── tools/              # Built-in tools (browser, exec, canvas, etc.)
│   ├── memory/             # Vector search, indexing, embeddings
│   ├── commands/           # CLI command implementations
│   └── cron/               # Cron job system
├── ui/                     # Control UI (Lit Web Components)
├── docs/                   # Documentation site (Mintlify)
├── apps/
│   ├── macos/              # macOS menu bar app (Swift)
│   ├── ios/                # iOS app (Swift)
│   └── android/            # Android app (Kotlin)
├── extensions/             # Optional plugins (Teams, Matrix, Voice Call, etc.)
├── skills/                 # Bundled AgentSkills
├── scripts/                # Build, test, release, e2e scripts
├── Dockerfile              # Container build
├── docker-compose.yml      # Full Docker deployment
├── CONTRIBUTING.md
├── CHANGELOG.md
└── VISION.md
```

### The architecture: Gateway as control plane

Everything flows through one process — the **Gateway**. It's a WebSocket server (default port 18789) that acts as the single control plane for your entire OpenClaw setup:

```
Messaging apps (WhatsApp / Telegram / Slack / Discord / Signal / iMessage / etc.)
        │
        ▼
┌───────────────────────────────────┐
│            Gateway                │
│    ws://localhost:18789           │
│    (single daemon process)        │
└──────────────┬────────────────────┘
               │
               ├── Pi Agent (embedded runtime — reasoning engine)
               ├── CLI (openclaw commands)
               ├── Web Chat / Control UI
               ├── macOS menu bar app
               └── iOS / Android nodes
```

The Gateway owns all messaging surfaces. It validates config via strict JSON Schema (generated from TypeBox). It runs the agent runtime in-process (not a separate service). It manages sessions, tools, cron jobs, heartbeats, and channel adapters.

**Key code paths:**
- `src/gateway/server.ts` — the Gateway server itself
- `src/agents/agent-pi.ts` — the AI agent executor
- `src/config/zod-schema.ts` — config validation schema
- `src/memory/manager.ts` — memory system (vector search via sqlite-vec)
- `src/tools/` — every built-in tool (browser control, bash execution, canvas, etc.)

The codebase is TypeScript chosen deliberately for hackability — it's an orchestration system (prompts, tools, protocols, integrations), and TypeScript makes it accessible to the largest number of contributors. Rust tooling handles the performance-critical dev pipeline: `oxlint` for linting, `oxfmt` for formatting.

## Config: the openclaw.json system

Your config lives at `~/.openclaw/openclaw.json`. It's JSON5 (supports comments and trailing commas). If the file doesn't exist, OpenClaw uses safe defaults — config is technically optional.

But here's the thing that trips people up: **validation is strict**. The config must match the Zod schema or the Gateway refuses to start. Unknown keys? Won't boot. Typo in a field name? Won't boot. Run `openclaw doctor` to diagnose.

### Key config sections

```json5
{
  // Gateway infrastructure
  gateway: { port: 18789, bind: "loopback", auth: { mode: "token" } },

  // Agent defaults
  agents: {
    defaults: { workspace: "~/.openclaw/workspace", model: "anthropic/claude-opus-4-6" },
    list: []  // multi-agent definitions go here
  },

  // Messaging channels
  channels: {
    telegram: { enabled: true, token: "..." },
    whatsapp: { enabled: true },
    // slack, discord, signal, imessage, googlechat, webchat, etc.
  },

  // Scheduled jobs
  cron: {},

  // Tool settings
  tools: { profile: "default" },

  // Auto-updater
  update: { channel: "stable" }
}
```

### Editing config

Five ways, pick your comfort level:

1. **`openclaw onboard`** — full interactive wizard (best for beginners)
2. **`openclaw configure`** — targeted config wizard
3. **`openclaw config get/set/unset`** — CLI one-liners for quick changes
4. **Control UI** — form editor + raw JSON at `http://localhost:18789/`
5. **Direct file edit** — the Gateway watches the file and hot-reloads

Hot-reload handles most changes (channels, agents, tools, cron). Infrastructure changes (port, bind, authentication, TLS) require a full restart.

### Your file structure on disk

```
~/.openclaw/
├── openclaw.json              # Main config
├── credentials/               # API keys (chmod 600)
│   ├── anthropic
│   └── openrouter
├── workspace/                 # Default agent workspace
│   ├── AGENTS.md              # Operating instructions
│   ├── SOUL.md                # Persona and tone
│   ├── USER.md                # Who the user is
│   ├── TOOLS.md               # Tool usage notes
│   ├── IDENTITY.md            # Agent name, emoji, vibe
│   ├── HEARTBEAT.md           # Heartbeat checklist
│   ├── MEMORY.md              # Curated long-term memory
│   └── memory/                # Daily logs (YYYY-MM-DD.md)
└── agents/<agentId>/          # Per-agent state (multi-agent mode)
```

Everything in the workspace is markdown. Plain text files you can open in any editor, version with git, and back up to a private repo. This is deliberate — your OpenClaw agent's memory and personality live in files you fully own and control.

## Authentication: API keys vs OAuth

Two paths to connect your AI model:

### API keys (simplest)

Get a key from your provider and store it:

```bash
# Environment variable
export ANTHROPIC_API_KEY="sk-ant-..."

# Or for daemon use, store in ~/.openclaw/.env
# Or set via onboard wizard
openclaw models status  # verify it works
```

OpenClaw supports multiple API keys per provider with automatic rotation on rate limits. Key priority: `OPENCLAW_LIVE_<PROVIDER>_KEY` → `<PROVIDER>_API_KEYS` → `<PROVIDER>_API_KEY`.

**Supported providers:** [Anthropic](https://console.anthropic.com/) (Claude), OpenAI ([ChatGPT](https://chat.openai.com/)/GPT/Codex), Google [Gemini](https://deepmind.google/technologies/gemini/), OpenRouter, xAI (Grok), Groq, Cerebras, Mistral, MiniMax, Moonshot, and more. You can also point it at any OpenAI-compatible API endpoint.

### OAuth (subscription-based)

If you have a Claude Pro/Max or ChatGPT Plus subscription, you can use OAuth instead of paying per-token:

```bash
# Anthropic OAuth
claude setup-token
openclaw models auth setup-token --provider anthropic

# OpenAI Codex OAuth
openclaw onboard  # choose openai-codex flow
```

Tokens are stored per-agent and auto-refresh on expiry. This is how many users keep costs down — use your existing subscription instead of raw API calls.

## Production vs beta: release channels

OpenClaw ships three release channels:

| Channel | npm tag | What it is | Who it's for |
|---------|---------|------------|--------------|
| **stable** | `latest` | Tagged releases, tested | Everyone |
| **beta** | `beta` | Prerelease builds under test | Early adopters |
| **dev** | `dev` | Head of `main` branch | Contributors |

Builds flow: `main` → beta → promoted to stable. The npm dist-tags are the source of truth, not git branches.

**How to check your channel:**
```bash
openclaw status
# or
npm view openclaw dist-tags
```

**How to switch channels:**
```bash
openclaw update --channel beta   # opt into beta
openclaw update --channel stable # back to stable
```

### How to update (without breaking everything)

This is the #1 pain point on X. People update and their paired devices break, agent tools stop working, or Raspberry Pi users get bricked. Here's the safe approach:

**Preferred method:**
```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

**Via npm:**
```bash
npm i -g openclaw@latest
# or
pnpm add -g openclaw@latest
```

**Critical step after any update:**
```bash
openclaw doctor
```

This migrates config schemas, audits DM policies, checks health, and catches issues before they become problems. **Always run `openclaw doctor` after updating.** This is the single most important thing beginners miss.

**Auto-updater (optional):**
```json5
{
  update: {
    channel: "stable",
    auto: {
      enabled: true,
      stableDelayHours: 6,    // wait before applying
      stableJitterHours: 12   // randomize to avoid thundering herd
    }
  }
}
```

Most experienced users leave auto-update off and update manually after checking the [CHANGELOG](https://github.com/openclaw/openclaw/blob/main/CHANGELOG.md) and community reports.

## Deploying on a VPS or Docker

Running OpenClaw on your laptop works, but it sleeps when you close the lid. For an always-on AI assistant, you want a server.

### VPS deployment

The most common production setup. Popular choices: Hetzner, DigitalOcean, any Linux box.

```bash
# SSH into your server
ssh user@your-vps

# Install Node.js 22+
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo bash -
sudo apt install -y nodejs

# Install OpenClaw
curl -fsSL https://openclaw.ai/install.sh | bash

# Run onboard wizard
openclaw onboard --install-daemon

# Verify
openclaw gateway status
```

The `--install-daemon` flag registers OpenClaw as a systemd service on Linux. It auto-starts on boot, restarts on crash.

**Remote access:** Never expose the Gateway port publicly. Use SSH tunneling or Tailscale:

```bash
# SSH tunnel (from your local machine)
ssh -N -L 18789:127.0.0.1:18789 user@your-vps

# Then open http://localhost:18789 in your browser
```

### Docker deployment

Full containerized setup:

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw
./docker-setup.sh
```

Or manually:

```bash
docker build -t openclaw:local -f Dockerfile .
docker compose run --rm openclaw-cli onboard
docker compose up -d openclaw-gateway
```

Docker is also used for **agent sandboxing** — run the Gateway on the host but isolate tool execution (bash, file access, browser) inside containers. Enable with `agents.defaults.sandbox.mode: "non-main"`.

### macOS (Mac Mini)

The community favorite. Many users run OpenClaw on a dedicated Mac Mini:

```bash
# Install via npm
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

The daemon registers with launchd and survives reboots. The [macOS](https://www.apple.com/macos/) menu bar app adds Voice Wake, Push-to-Talk, and quick access to the web chat dashboard.

## Multi-agent routing

This is where OpenClaw gets powerful — and complex. You can run multiple isolated agents inside a single Gateway, each with its own workspace, personality, and connected messaging apps.

### Setting up multiple agents

```bash
openclaw agents add coding    # create a coding-focused agent
openclaw agents add social    # create a social media agent
openclaw agents list --bindings
```

Each OpenClaw agent gets its own:
- **Workspace** (separate AGENTS.md, SOUL.md, memory)
- **State directory** (auth profiles, model preferences)
- **Session store** (conversation history)

### Routing rules

Routing is deterministic, most-specific-wins:

1. `peer` match (exact DM or group ID)
2. `parentPeer` match (thread inheritance)
3. `guildId + roles` (Discord role-based routing)
4. `guildId` / `teamId`
5. `accountId`
6. Channel-level match
7. Fallback to default agent

You can route different WhatsApp contacts to different agents, have Discord roles map to specialized bots, or run entirely separate personalities on different channels — all from one Gateway.

### Sub-agents

Within a single agent, you can spawn isolated sub-agent sessions for parallel work. This is how people build AI agent teams: a coordinator agent delegates tasks to specialized sub-agents that run concurrently, report back, and get cleaned up.

The workflows people build with this are wild — content creation pipelines, research teams, code review squads, all orchestrated through cron jobs and messaging apps.

## Cron and heartbeat: the automation engine

Two systems for scheduled work, and the difference confuses almost everyone.

### Heartbeat

A periodic check-in on your **main session**. Default: every 30 minutes (1 hour for OAuth auth). Reads `HEARTBEAT.md` for a checklist of things to monitor. If nothing needs attention, the agent responds `HEARTBEAT_OK` (suppressed — you never see it). If something's wrong, it sends you a notification.

Think of heartbeat as your agent's "pulse check" — batched, context-aware, running in your existing conversation.

### Cron

Precise scheduling with full cron expressions. Each job runs in its own isolated session. You can set different models per job, delivery targets, and one-shot reminders.

```bash
# Morning briefing at 7am ET, delivered to WhatsApp
openclaw cron add \
  --name "Morning briefing" \
  --cron "0 7 * * *" \
  --tz "America/New_York" \
  --session isolated \
  --message "Give me today's briefing: calendar, priority emails, weather" \
  --announce \
  --channel whatsapp

# One-shot reminder in 20 minutes
openclaw cron add \
  --name "Reminder" \
  --at "20m" \
  --session main \
  --system-event "Remind me to call the dentist" \
  --delete-after-run
```

**Cost tip from the community:** Use cheap models for simple cron tasks (notifications, data fetching) and expensive models for interactive work. One user runs GLM for daily cron jobs and Claude Opus for conversations — meaningful cost savings on automation that doesn't need frontier intelligence.

## Security and the allowlist model

OpenClaw's security works in three layers:

### 1. Sandbox (WHERE tools run)

```json5
{ agents: { defaults: { sandbox: { mode: "non-main" } } } }
```
- `off` — everything runs on host (default)
- `non-main` — isolated sessions run in Docker
- `all` — everything sandboxed

### 2. Tool policy (WHICH tools are available)

```json5
{
  tools: {
    profile: "messaging",  // preset
    deny: ["group:automation", "group:runtime"],  // deny always wins
    exec: { security: "deny", ask: "always" }
  }
}
```

Use the allowlist approach: start restrictive, open up as needed. `deny` always beats `allow`.

### 3. DM/Group access (WHO can talk to your agent)

```json5
{
  channels: {
    whatsapp: {
      dmPolicy: "pairing",     // require approval code (default)
      // or: "allowlist"       // only pre-approved senders
      // or: "open"            // anyone can DM (dangerous)
    }
  }
}
```

**Run the security audit:**
```bash
openclaw security audit --deep --fix
```

This checks inbound access, tool permissions, network exposure, browser control, file permissions, and policy drift. Do this after any major config change.

## Connecting messaging apps

OpenClaw supports a staggering number of channels. Here's the practical setup for the most common ones:

| Channel | Library | Setup |
|---------|---------|-------|
| **Telegram** | grammY | Create bot via @BotFather, paste token |
| **WhatsApp** | Baileys | Scan QR code, links to your WhatsApp account |
| **Slack** | Bolt | Create Slack app, configure OAuth scopes |
| **Discord** | discord.js | Create bot in Discord Developer Portal |
| **Signal** | signal-cli | Install signal-cli (JVM or native), link or register number |
| **iMessage** | BlueBubbles/imsg | macOS only, requires Full Disk Access |
| **Web Chat** | Built-in | Already running at localhost:18789 |

**General pattern:**
1. Add channel config to `openclaw.json`
2. Restart: `openclaw gateway restart`
3. Approve pairing: `openclaw pairing approve <channel> <code>`
4. Verify: `openclaw channels status --probe`

Telegram is the most popular choice — straightforward setup, reliable, works on every platform. The web chat at `http://localhost:18789` requires zero setup and is great for testing.

## The CLI reference

OpenClaw's CLI is extensive. Here are the commands you'll actually use:

```bash
# Setup & maintenance
openclaw onboard              # Full setup wizard
openclaw doctor               # Diagnose + repair (RUN THIS AFTER UPDATES)
openclaw dashboard             # Open Control UI in browser
openclaw status                # System status
openclaw gateway start|stop|restart|status

# Config
openclaw config get agents.defaults.model
openclaw config set channels.telegram.enabled true
openclaw configure             # Interactive config editor

# Agents
openclaw agents list --bindings
openclaw agents add <name>
openclaw agent --message "do something"

# Channels
openclaw channels status --probe
openclaw channels login <channel>

# Models
openclaw models list
openclaw models set anthropic/claude-opus-4-6

# Cron
openclaw cron list
openclaw cron add --name "..." --cron "..." --message "..."
openclaw cron run <jobId>

# Memory
openclaw memory status
openclaw memory search "query"

# Logs & debugging
openclaw logs --follow
openclaw security audit --deep
```

## Building from source

For contributors or anyone who wants to run the development branch:

```bash
git clone https://github.com/openclaw/openclaw.git
cd openclaw
pnpm install
pnpm ui:build      # builds the Control UI
pnpm build         # compiles TypeScript → dist/

# Run directly (no build needed for dev)
pnpm openclaw onboard --install-daemon

# Auto-reload dev loop
pnpm gateway:watch
```

**Prerequisites:** Node.js 22+, pnpm 10.23+ (via corepack), Git 2.x. For native apps: Xcode 26.1 (macOS/iOS), Android SDK API 36 (Android).

**Dev workflow:** `pnpm openclaw ...` runs TypeScript directly via `tsx` — no build step needed during development. `pnpm build` produces the packaged `dist/` for release.

**Pre-PR checklist:**
```bash
pnpm build && pnpm check && pnpm test
```

## Contributing

From the [CONTRIBUTING.md](https://github.com/openclaw/openclaw/blob/main/CONTRIBUTING.md):

- **Bugs and small fixes** → open a PR directly
- **New features or architecture changes** → start a GitHub Discussion or ask in Discord first
- **AI-assisted PRs are explicitly welcome** — mark them, note testing level, include prompts if possible
- **Keep PRs focused** — one thing per PR, describe what and why
- **Current priorities:** stability (channel edge cases), UX (onboarding, error messages), skills (via ClawHub), performance (token usage, compaction)

The maintainer team has 12+ people covering different areas: [Steipete](https://x.com/steipete) as BDFL, plus dedicated maintainers for Discord, Telegram, iOS, macOS, agents, security, docs, CLI, web UI, memory, and more.

---

If you starred the repo but never looked inside, now you know what's there. OpenClaw is a chatbot that became an AI assistant that became a platform — and the [GitHub.com](https://github.com/openclaw/openclaw) repo reflects that evolution. It's big, it moves fast, and it rewards people who take the time to understand it.

Start with `openclaw onboard`. Run `openclaw doctor` after every update. And back up your workspace to a private git repo — your agent's memory is worth protecting.

🦞

---

*Sources: [OpenClaw GitHub](https://github.com/openclaw/openclaw), [docs.openclaw.ai](https://docs.openclaw.ai), [CONTRIBUTING.md](https://github.com/openclaw/openclaw/blob/main/CONTRIBUTING.md), community reports on X*
