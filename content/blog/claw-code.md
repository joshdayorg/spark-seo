---
title: "Claw Code: The Complete Guide to Claude Code, Anthropic's AI Coding Agent"
date: "2026-02-22"
author: "JD"
authorTitle: "Founder, Spark"
authorImage: "/images/jd-avatar.jpg"
authorLinkedIn: "https://linkedin.com/in/joshuaday"
category: "AI Tools"
featured: false
excerpt: "People search 'claw code' and mean three different things. Here's the definitive guide to Claude Code — what it is, how to use it, what it costs, and why it keeps getting confused with OpenClaw."
coverImage: "https://images.ctfassets.net/placeholder-claw-code.jpg"
hideTitle: false
---

# Claw Code: The Complete Guide to Claude Code, Anthropic's AI Coding Agent

## Why people search "claw code"

If you Googled "claw code" and ended up here, you're not alone. The term means different things to different people right now:

1. **Claude Code** — Anthropic's agentic coding tool that reads your codebase, edits files, and runs commands. This is what most people mean.
2. **"Claw Code" as slang for Claude Code running inside OpenClaw** — the open source AI assistant originally called Clawdbot. Japanese and Chinese dev communities especially use "claw code" this way.
3. **ClawCode** — a community project by [@iamAliAgha](https://x.com/iamAliAgha) focused on shipping production features with ai agents.

This guide covers the first one — Claude Code by Anthropic — because that's what 90% of "claw code" searchers actually want. If you're looking for OpenClaw, we wrote [the full explainer here](/blog/what-is-openclaw-ai).

## What Claude Code actually is

Claude Code is an ai coding tool built by [Anthropic](https://www.anthropic.com) that operates as a full coding agent, not just an autocomplete. You describe what you want in plain language. It reads your files, plans changes across multiple files, writes the code, runs shell commands, checks its own work, and iterates until the job is done.

It launched in February 2025 as a research preview. By December 2025 it had gone viral — non-programmers were using it for "vibe coding," NASA used it to [plan a route for the Perseverance Mars rover](https://www.anthropic.com/news/claude-code-plugins), and Microsoft, Google, and (briefly) OpenAI employees were all using it internally. It just turned one year old, and its creator [Boris Cherny](https://www.linkedin.com/in/boris-cherny/) celebrated the milestone on X to massive engagement.

The tool is available everywhere: terminal cli, VS Code, JetBrains, a standalone desktop app, a web version at [claude.ai/code](https://claude.ai/code), and even the Claude iOS app. You can start a session on your laptop, `/teleport` it to the web, and keep working from your phone.

## Getting started: a quick tutorial for beginners

### Install the CLI

**macOS or Linux:**
```bash
curl -fsSL https://claude.ai/install.sh | bash
```

**Windows (PowerShell):**
```powershell
irm https://claude.ai/install.ps1 | iex
```

**Homebrew:**
```bash
brew install --cask claude-code
```

Then open any project directory and type `claude`. That's it. You're in.

```bash
cd your-project
claude
```

You'll authenticate through [claude.ai](https://claude.ai) on first run. No api key needed if you're on a Pro, Max, or Team plan. If you want api access instead, set `ANTHROPIC_API_KEY` in your environment.

### IDE setup

If you prefer working inside an editor rather than the terminal:

- **VS Code / Cursor**: Search "Claude Code" in Extensions and install. You get inline diffs, @-mentions for file context, and conversation history in the sidebar.
- **JetBrains** (IntelliJ, PyCharm, WebStorm): Install from the [JetBrains Marketplace](https://plugins.jetbrains.com/plugin/27310-claude-code-beta-). Still in beta but fully functional.

Both ide integrations give you the same agentic capabilities as the cli — file editing, command execution, plan review — just wrapped in your editor's UI.

### Your first real task

Don't start with "build me a full app." Start with something your codebase already needs:

```
claude "find the bug in auth.py and fix it"
claude "add input validation to the signup form"
claude "write tests for the payment module"
```

Claude Code reads your project, identifies relevant files, makes changes, and runs your test suite to verify. You review the diff and accept or reject.

## How the workflow actually works

### CLAUDE.md: the new readme for AI

Drop a `CLAUDE.md` file in your project root. Claude Code reads it at the start of every session. This is where you put coding standards, architecture decisions, preferred libraries, things not to touch.

Boris Cherny — the coder who built Claude Code — shared his personal CLAUDE.md template and it went viral. [222 retweets in the Japanese dev community alone](https://x.com/masahirochaen). The concept of a project-level instruction file for your ai coding agent is becoming standard practice fast.

Think of it as a readme, but for your AI. The workspace configuration that tells Claude Code how your project works before it writes a single line.

### Skills: teaching Claude Code new tricks

Skills are markdown instruction files you drop into `.claude/skills/` in your project or `~/.claude/skills/` globally. Each one gives Claude Code a new capability — a deployment checklist, a code review workflow, a database migration pattern. They follow the open [Agent Skills standard](https://agentskills.io), so skills built for Claude Code also work with other ai tools that support the spec.

These coding skills extend what Claude Code can do without writing any actual code yourself. Just describe the process in markdown, and Claude Code follows it.

### Hooks: automation at every step

Hooks are shell commands that fire at specific lifecycle points — before a tool runs, after a session starts, when a subagent spins up. You can use them to auto-format code after every edit, block dangerous commands, enforce lint rules, or trigger deploys.

```json
{
  "hooks": {
    "PostToolUse": [{
      "matcher": { "tool_name": "Write" },
      "command": "prettier --write $CLAUDE_FILE_PATH"
    }]
  }
}
```

This is where real workflow automation happens. Not in a separate CI system — right inside the coding loop.

### Agent teams and subagents

Claude Code can spawn multiple ai agents working on different parts of a problem simultaneously. Two patterns:

**Subagents** are fire-and-forget workers. Claude Code delegates a focused task (write tests for this module, research this API), gets a summary back, and keeps going. Lower token cost, simpler.

**Agent teams** (experimental) are coordinated groups that share task lists, message each other, and self-organize under a lead agent. Boris Cherny runs [10-15 parallel Claude Code sessions](https://x.com/heygurisingh) as his daily workflow. This is the pattern power users are converging on.

## What it costs: pricing breakdown

### Subscription plans (recommended for most people)

| Plan | Price | What you get |
|------|-------|-------------|
| Free | $0 | Chat only, no Claude Code |
| Pro | $20/mo | Claude Code included, standard usage |
| Max 5x | $100/mo | 5x Pro usage, priority access |
| Max 20x | $200/mo | 20x Pro usage |
| Team | $25/seat/mo | SSO, admin controls, collaboration |
| Enterprise | Custom | HIPAA-ready, SCIM, audit logs |

### API pricing (per million tokens)

| Model | Input | Output |
|------|-------|--------|
| Claude Opus 4.6 | $5 | $25 |
| Claude Sonnet 4.5 | $3 | $15 |
| Claude Haiku 4.5 | $1 | $5 |

Opus is the most capable model — best for complex reasoning, large refactors, ambiguous instructions. Sonnet handles most day-to-day coding at 40% less cost. The batch api gives an additional 50% discount for async workloads.

For context: [Peter Steinberger](https://steipete.me) (creator of [OpenClaw](https://openclaw.ai)) was reportedly spending $10K/month running AI agents before joining OpenAI. Most individual developers on a Pro or Max plan spend far less — the subscription caps your spend.

## The SDK: building custom agents

The Claude Agent SDK (formerly Claude Code SDK) lets you build your own ai agents programmatically using the same tools Claude Code uses internally. Available in Python and TypeScript.

```bash
npm install @anthropic-ai/claude-agent-sdk
```

```python
from claude_agent_sdk import query, ClaudeAgentOptions

async for message in query(
    prompt="Find and fix the bug in auth.py",
    options=ClaudeAgentOptions(allowed_tools=["Read", "Edit", "Bash"]),
):
    print(message)
```

The sdk gives you access to all built-in tools — file read/write, bash execution, web search, web fetch, grep, glob — plus hooks, subagent orchestration, and MCP integration. It's open source and MIT licensed.

Use cases people are building with it: automated code review pipelines, financial compliance agents, security scanning workflows, and CI bots that fix failing tests autonomously.

## Integrations that matter

### Slack

The [Slack integration](https://support.claude.com/en/articles/11506255-getting-started-with-claude-in-slack) lets you @mention Claude in any channel with a coding request. It detects coding intent, spins up a web Claude Code session, posts progress to the Slack thread, and delivers the result with buttons to view the session or create a PR. Available on Team and Enterprise plans.

### GitHub Actions

The [official GitHub Action](https://github.com/anthropics/claude-code-action) lets you @mention `@claude` in any PR or issue. Claude Code analyzes the context, implements changes, and opens a PR. Run `/install-github-app` inside Claude Code to set it up. The template generates a GitHub Actions workflow file automatically.

### OpenClaw

This is where "claw code" comes full circle. OpenClaw — the open source personal AI assistant that started as Clawdbot — can spawn and manage Claude Code sessions remotely. Users trigger coding tasks from their phone via Telegram or WhatsApp, and OpenClaw orchestrates Claude Code on their dev machine in the background. It's why "claw code" became a thing — the two tools are deeply intertwined in practice.

## Claude Code vs Codex: the real rivalry

As of February 2026, Codex (OpenAI) is the main competitor. Cursor has faded from direct comparisons.

The consensus from experienced users: **code quality is roughly equivalent**. [@chrisalbon](https://x.com/chrisalbon) (notable ML engineer) tested both extensively and found results "well within the margin of error."

Where they differ:
- Claude Code has a larger context window (200K tokens, 1M in beta) vs Codex's smaller window
- Codex is reportedly faster on simple tasks
- Claude Code has deeper integration with the broader Claude ecosystem (claude.ai, MCP, Slack)
- A viral technique emerged where developers [tell Claude Code that Codex is reviewing its output](https://x.com/yadong_xie) — and it performs better. The "competitive prompting" trick got 89 retweets

Japanese developers have pioneered a pipeline where Codex automatically reviews Claude Code's plans before execution using `codex --resume --last`. Best of both worlds.

## The roadmap: what's new and what's next

Claude Code just hit its one-year anniversary with a strong 2026 so far:

- **Claude Code Security** (Feb 2026) — scans codebases for vulnerabilities. The announcement got [5,710 retweets](https://x.com/claudeai) and reportedly crashed $15 billion in cybersecurity stocks
- **Opus 4.6 support** (Feb 2026) — the most capable model yet, with a 14.5-hour autonomous task horizon
- **Agent teams** — experimental multi-agent coordination
- **Plugins ecosystem** — 50+ official plugins in the marketplace, with a community hub at [claudepluginhub.com](https://www.claudepluginhub.com/)
- **Claude Agent SDK** — renamed from Claude Code SDK, now available in Python and TypeScript
- **500-person hackathon** — Anthropic just wrapped a week-long Claude Code hackathon with Opus 4.6

The docs are comprehensive and actively maintained at [code.claude.com/docs](https://code.claude.com/docs/en/overview). The GitHub repo at [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code) has 68.9K stars and growing.

## Resources

| Resource | Link |
|----------|------|
| Claude Code docs | [code.claude.com/docs](https://code.claude.com/docs/en/overview) |
| Claude Code on GitHub | [github.com/anthropics/claude-code](https://github.com/anthropics/claude-code) |
| Claude Agent SDK | [platform.claude.com/docs/en/agent-sdk](https://platform.claude.com/docs/en/agent-sdk/overview) |
| Claude Code plugins | [github.com/anthropics/claude-code/plugins](https://github.com/anthropics/claude-code/tree/main/plugins) |
| Plugin Hub (community) | [claudepluginhub.com](https://www.claudepluginhub.com/) |
| GitHub Action | [github.com/anthropics/claude-code-action](https://github.com/anthropics/claude-code-action) |
| Pricing | [claude.com/pricing](https://claude.com/pricing) |
| VS Code Extension | [VS Code Marketplace](https://marketplace.visualstudio.com/items?itemName=anthropic.claude-code) |
| JetBrains Plugin | [JetBrains Marketplace](https://plugins.jetbrains.com/plugin/27310-claude-code-beta-) |
| Slack setup guide | [support.claude.com](https://support.claude.com/en/articles/11506255-getting-started-with-claude-in-slack) |
| Boris Cherny (creator) | [LinkedIn](https://www.linkedin.com/in/boris-cherny/) |
| OpenClaw (the "claw" connection) | [openclaw.ai](https://openclaw.ai) |
