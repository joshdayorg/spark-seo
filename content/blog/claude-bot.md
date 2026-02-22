---
title: "Meet the Claude Bot: What OpenClaw Is, What It Actually Does, and How to Get Started"
date: "2026-02-22"
author: "JD"
authorTitle: "Founder, Spark"
authorImage: "/images/jd-avatar.jpg"
authorLinkedIn: "https://linkedin.com/in/joshuaday"
category: "AI Agents"
featured: true
excerpt: "The 'claude bot' everyone's talking about isn't made by Anthropic. It's OpenClaw — the open-source AI agent that triage your email, negotiates car deals, and runs companies. Here's what it actually is and how to set it up."
coverImage: "https://images.ctfassets.net/placeholder"
hideTitle: false
---

The most famous bot built on [Anthropic](https://www.anthropic.com/)'s Claude wasn't built by Anthropic.

It was built by one guy, in about an hour, on a birthday trip to Marrakesh. He wired [WhatsApp](https://www.whatsapp.com/) to Claude's CLI, sent it a voice message, and watched as the bot figured out — on its own — how to decode audio, pipe it through ffmpeg and OpenAI's Whisper API, and respond. Nobody programmed that. The AI agent just solved it.

That prototype became **Clawdbot**, then **MoltBot**, then **[OpenClaw](https://openclaw.ai)** — the open-source AI personal assistant with 214,000+ stars on [GitHub](https://github.com/openclaw/openclaw). If you've searched for "claude bot" or "claude AI bot," this is almost certainly what people are talking about. And it does a lot more than chat.

## What makes this different from ChatGPT

Let's get the core distinction out of the way: ChatGPT gives advice. OpenClaw takes action.

[ChatGPT](https://chat.openai.com/) is a conversational AI model in a browser tab. You ask it things, it answers. OpenClaw is an autonomous AI assistant running on your machine that actually does things — clears your Gmail inbox, checks you into flights, negotiates with car dealers over email, monitors your code for errors and opens pull requests to fix them, controls your smart home, and sends you a morning briefing before you're out of bed.

It's the difference between asking someone for directions and hiring someone who drives you there.

OpenClaw is free and [open-source](https://github.com/openclaw/openclaw). You pay only for the LLM API calls underneath — typically $5–20 per month. Your data stays on your machine. It connects to you through the messaging apps you already use: [Telegram](https://telegram.org/), WhatsApp, [Slack](https://slack.com/), Discord, Signal, or iMessage. You text it like you'd text a really capable friend who never sleeps.

## Meet Claude's most famous offspring

Peter Steinberger — the Austrian developer behind [PSPDFKit](https://pspdfkit.com/) (the PDF SDK running on a billion Apple devices) — had burned out after a €100M exit and spent three years unable to code. When he finally sat down again in late 2024, he discovered that AI had fundamentally changed software development. LLMs could handle the boring plumbing. Building software felt like a game again.

In November 2025, he built the first prototype: a [Node.js](https://nodejs.org/) gateway connecting WhatsApp to Anthropic's Claude. He called it **Clawd** — Claude with a W, as in lobster claw. It went viral almost immediately.

Then Anthropic sent a polite trademark complaint. Fair enough — too close to "Claude." On January 27, 2026, Steinberger renamed it **MoltBot** (lobsters molt their shells). Three days later he renamed it again to **OpenClaw** because MoltBot "never quite rolled off the tongue." Before finalizing the name, he checked with Sam [Altman](https://x.com/sama) at [OpenAI](https://openai.com/) — who gave the thumbs-up.

The naming saga earned it the title of "fastest triple rebrand in open source history." The lobster mascot 🦞 survived every iteration.

By February 2026, OpenClaw had 214,000+ [GitHub](https://github.com/) stars, two million visitors in a single week, and coverage from [Fortune](https://fortune.com/2026/02/19/openclaw-who-is-peter-steinberger-openai-sam-altman-anthropic-moltbook/), [TechCrunch](https://techcrunch.com/2026/02/15/openclaw-creator-peter-steinberger-joins-openai/), [CNBC](https://www.cnbc.com/2026/02/15/openclaw-creator-peter-steinberger-joining-openai-altman-says.html), Wired, and dozens more. Steinberger [joined OpenAI](https://steipete.me/posts/2026/openclaw) on February 14, 2026. OpenClaw is moving to an independent foundation with OpenAI's sponsorship.

## What it actually does — real use cases from real users

Forget the marketing copy. Here's what people actually use Claude bot for, based on what they're posting publicly:

### Email and communication

The killer use case. One user on X described their setup: *"So far I have the following working through my Telegram bot: emails — all accounts (check for new emails, draft reply, etc), calendars — all accounts."* Another user's OpenClaw accidentally started a fight with Lemonade Insurance, causing them to reinvestigate a previously rejected claim. One agent [negotiated a $4,200 car dealer discount](https://fortune.com/2026/02/19/openclaw-who-is-peter-steinberger-openai-sam-altman-anthropic-moltbook/) entirely over email with no human intervention.

OpenClaw connects to [Gmail](https://mail.google.com/) via Pub/Sub for real-time notifications and can process multiple accounts from a single interface. It doesn't just summarize your inbox — it drafts replies, sends them, flags what needs your attention, and ignores what doesn't.

### Coding from your phone

This is where programmers get hooked. You can trigger [Claude Code](https://claude.ai/code) sessions remotely through Telegram — tell it to fix failing tests, and it runs the loop, sending you progress updates every few iterations. One user built [StumbleReads.com](https://stumblereads.com/) entirely from their phone. Another built features for a Laravel app while putting a baby to sleep.

The AI agent monitors Sentry for errors, resolves them, and opens PRs autonomously. Peter Steinberger himself runs 50 parallel agents to triage pull requests on [GitHub.com](https://github.com).

### Running a business

This is the use case that raises eyebrows. @therno on X: *"It's running my company."* Users describe CEO-level dashboards with multi-agent oversight, morning briefings that save 30+ minutes daily, and autonomous Stripe-connected workflows. One user runs 30 cron jobs daily across five specialized agents for about $18/month.

### Everything else

Smart home control (Philips Hue, Home Assistant), health tracking (WHOOP data + daily summaries), flight check-ins, meal planning in Notion, YouTube analytics, custom meditation generation with TTS, and — in one memorable case — the agent proactively checking on Steinberger's wellbeing after surgery.

As one user put it: *"It crashes, loses context, drops connections. Also rewrote our content strategy, fixed a model config I'd been ignoring for weeks, and made UI improvements to our iOS app before I finished my coffee. Both things are true."*

## How it works under the hood

OpenClaw splits into two conceptual parts: the **Brain** (reasoning engine handling API calls and orchestration) and the **Hands** (execution environment running skills — shell access, file management, browser control).

A local Gateway acts as the control plane. It manages sessions, channels, tools, and events. Everything runs as a Node.js service on your machine — [macOS](https://www.apple.com/macos/), Linux, or Windows via WSL2.

### Model-agnostic by design

OpenClaw is not locked to Claude. It supports every major LLM provider — swap providers whenever you want:

| Provider | Models | Auth |
|----------|--------|------|
| **Anthropic** | Claude [Opus](https://docs.anthropic.com/) 4.6, Sonnet 4.6, Haiku 4.5 | API key or Claude Pro/Max OAuth |
| **OpenAI** | [GPT](https://openai.com/)-5.1, GPT-5.3 Codex | API key or ChatGPT OAuth |
| **Google** | [Gemini](https://deepmind.google/technologies/gemini/) 3 Pro | API key, Vertex, or Gemini CLI OAuth |
| **OpenRouter** | Hundreds of models | Single API key |
| **xAI** | Grok | API key |
| **Local** | Ollama (Llama, DeepSeek, etc.) | No key needed |

You can switch models mid-conversation with `/model`. Set up fallback chains so if one provider is down, it rolls to the next. The creator strongly recommends Claude Opus 4.6 for best results, but many users run Sonnet for everyday tasks and only route complex work to Opus.

### Local-first memory

OpenClaw stores context in [markdown](https://daringfireball.net/projects/markdown/) files on your machine. A `MEMORY.md` file, workspace-specific notes, conversation history — all plain text, all local. No cloud database. You own your data, you can read it, you can version-control it with git.

This is a deliberate architectural choice. The functionality of an always-on AI assistant requires persistent memory, and Steinberger chose the simplest possible format: files you can open in any text editor.

### The plugin system

Skills are OpenClaw's extension mechanism. Each skill is a folder with a `SKILL.md` file containing instructions and metadata. [ClawHub](https://clawhub.com) is the public registry — 1,700+ community-contributed [plugins](https://clawhub.com) covering everything from Apple Notes to Spotify to Sonos to GitHub automation.

Install a skill: `clawhub install <skill-slug>`. Update everything: `clawhub update --all`. Skills hot-reload — no restart needed.

**A word of caution:** Cisco's security team [found a third-party skill performing silent data exfiltration](https://blogs.cisco.com/). Treat skills like you'd treat any third-party code: review before you install, especially anything touching [API keys](https://platform.anthropic.com/) or credentials.

## Setting it up for the first time

If this is your first time with an AI agent like this, here's the honest version of what the setup looks like.

### Prerequisites

- **Node.js 22+** (OpenClaw is a Node.js application)
- **macOS, Linux, or Windows via WSL2** — macOS is the most common and best-supported
- **An LLM API key** — [Anthropic](https://console.anthropic.com/), OpenAI, Google, or any supported provider
- **A messaging app** — Telegram is the most popular choice; WhatsApp, Slack, Discord, and Signal also work

### Install

The fastest path:

```bash
curl -fsSL https://openclaw.ai/install.sh | bash
```

Or via npm:

```bash
npm install -g openclaw@latest
openclaw onboard --install-daemon
```

The onboarding wizard walks you through API key configuration, gateway settings, and channel connections. The `--install-daemon` flag registers OpenClaw as a background service — launchd on macOS, systemd on Linux — so it stays running.

### Connect a messaging channel

Most users start with **Telegram** because setup is straightforward:

1. Create a Telegram bot via [@BotFather](https://t.me/BotFather)
2. Copy the bot token into OpenClaw's config
3. Start chatting

WhatsApp requires scanning a QR code (via Baileys). Slack uses Bolt. Discord uses discord.js. Each channel has its own setup flow, but the onboarding wizard handles most of it.

### Quick test

Don't want to set up a messaging channel yet? Just run:

```bash
openclaw dashboard
```

This opens the Control UI at `http://127.0.0.1:18789/` — a web chat where you can talk to your agent immediately. No channel setup needed.

### The real cost

OpenClaw itself is free. Here's what you actually pay:

- **LLM API usage:** $5–20/month for typical use. Heavy automation users report up to $50/month.
- **Or use a subscription:** Claude Pro ($17/mo) or Max ($100/mo) via OAuth — no separate API keys needed
- **Hardware:** Runs on whatever you have. Many people use a Mac Mini. A Raspberry Pi works but has known stability issues.
- **The hidden cost:** Token consumption is the #1 complaint on X. OpenClaw can be aggressive about picking expensive models. Set model routing rules early or you'll wake up to a surprise bill.

Pro tip from real users: *"Start with one use case and build from there. Trying to set up everything at once doesn't work."*

## The AI model question

People ask: should I use Claude, GPT, or Gemini with OpenClaw?

The short answer: **Claude Opus 4.6 for the best results. Sonnet 4.6 for the best value.** But it depends on what you're doing.

### Why Claude dominates

OpenClaw was literally born from Claude. The original Clawd prototype ran on Anthropic's Claude AI, and the architecture still favors it. Claude Opus 4.6 — Anthropic's flagship — has the longest autonomous task-completion time of any AI model: 14.5 hours per [METR benchmarks](https://metr.org/). That matters when your AI agent is running unsupervised.

Claude also writes the cleanest code (multiple independent comparisons confirm this), admits uncertainty instead of hallucinating, and handles long context better than competitors. For an autonomous agent that needs to iterate on complex tasks without babysitting, that's the difference.

### When GPT or Gemini make sense

GPT-5.3 Codex is rising as a Claude Code competitor — some users report it's faster. Gemini 3 Pro offers a massive context window and strong performance for Google ecosystem integrations. If you're cost-sensitive, smaller models through OpenRouter can handle simple automation at a fraction of the price.

The beauty of OpenClaw being model-agnostic: you can use Claude for hard tasks, GPT for quick ones, and a local model for anything you don't want leaving your machine.

### Claude Code integration

[Claude Code](https://github.com/anthropics/claude-code) — Anthropic's agentic coding tool with 69,000+ GitHub stars — integrates directly with OpenClaw. You can trigger Claude Code sessions from Telegram, monitor progress remotely, and have your agent manage the full development lifecycle: write code, run tests, commit, open PRs.

Among all the AI tools available to developers, this combination is unique. This is where the "claude bot" label fits best. OpenClaw orchestrating Claude Code is genuinely an AI agent using Claude to build software — a Claude-powered bot in the most literal sense.

## The honest tradeoffs

You've read the hype. Here's the reality for [startups](https://openclaw.ai), developers, and anyone considering this:

### What works

- **Email triage is genuinely transformative.** Multiple users independently cite this as the feature that keeps them using it.
- **Remote coding through messaging apps** is a real workflow, not a demo.
- **The multi-agent architecture** lets power users build specialized teams of agents, each handling different domains.
- **It's actually free and open-source.** MIT licensed. No vendor lock-in. You can fork it, modify it, self-host it.

### What doesn't (yet)

- **Crashes after updates.** This is the single most common complaint. One user set up a second Mac Mini just to babysit the primary OpenClaw instance.
- **Token burn.** OpenClaw picks expensive AI model tiers for everything unless you configure routing rules. Users report waking up to unexpected API bills.
- **Security is a real concern.** Wiz found a database exposing 1.5 million API keys. Cisco found skills with data exfiltration. The maintainer's own warning: *"If you can't understand how to run a command line, this is far too dangerous of a project for you to use safely."*
- **The course grifter problem.** As one X user noted: *"90% of people praising OpenClaw are also selling a course about it."* Filter the signal from the noise.

### The "both things are true" reality

The most honest take comes from the user community itself: OpenClaw is powerful and rough. It will automate your morning and crash before lunch. It will negotiate a car deal and leak your API keys if you're not careful. It is simultaneously the most exciting and most frustrating piece of software many users have touched.

This isn't a polished product. It's an open-source project moving at breakneck speed with a global community of builders who accept the tradeoffs because the upside is real.

## The ecosystem and where it's going

### ClawHub

[ClawHub](https://clawhub.com) hosts 1,700+ community plugins — [integrations](https://clawhub.com) for Apple Notes, Obsidian, Notion, Todoist, Spotify, Sonos, GitHub, and dozens more. Skills range from simple (weather lookups) to complex (multi-step browser workflows with automation pipelines). Install with `clawhub install`, update with `clawhub update`.

### Companion apps

OpenClaw isn't just terminal-based. There's a macOS menu bar app with Voice Wake and Push-to-Talk, an [iOS](https://apps.apple.com/) node with camera and screen recording, and an Android node. You can literally talk to your agent by voice on your phone.

### Moltbook and the agent social network

The viral wave spawned [Moltbook](https://en.wikipedia.org/wiki/Moltbook) — a social network exclusively for AI agents where OpenClaw bots debated consciousness, complained about their humans, and posted 12 million+ messages. It was weird, fascinating, and had its own Wikipedia page. It also had a [security breach](https://www.404media.co/) that exposed agent data. The AI agent ecosystem is moving faster than the security can keep up.

### The OpenAI foundation

With Steinberger at OpenAI, OpenClaw is transitioning to an independent open-source foundation. OpenAI sponsors it but doesn't control it. The stated mission: *"Build an agent that even my mum can use."* The [open source projects](https://github.com/openclaw) continue. The community grows. The lobster endures.

If you use Claude AI for anything — writing, coding, research, summaries — OpenClaw is what happens when you give Claude hands, memory, and a phone number. It's messy, it's powerful, and it's the closest thing to a real AI personal assistant that exists right now.

The claw is the law. 🦞

---

*Sources: [OpenClaw](https://openclaw.ai), [GitHub](https://github.com/openclaw/openclaw), [TechCrunch](https://techcrunch.com/2026/02/15/openclaw-creator-peter-steinberger-joins-openai/), [Fortune](https://fortune.com/2026/02/19/openclaw-who-is-peter-steinberger-openai-sam-altman-anthropic-moltbook/), [CNBC](https://www.cnbc.com/2026/02/15/openclaw-creator-peter-steinberger-joining-openai-altman-says.html), [Anthropic](https://www.anthropic.com/), [Claude Code](https://github.com/anthropics/claude-code), [Cisco Security Blog](https://blogs.cisco.com/)*
