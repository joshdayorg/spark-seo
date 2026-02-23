# Web Research: OpenClaw CLI Tools, Skills System & MCP Comparison

## Sources (8+ articles read in full)

1. Official docs: [skills](https://docs.openclaw.ai/tools/skills), [agent-workspace](https://docs.openclaw.ai/concepts/agent-workspace)
2. Luke Tucker: ["OpenClaw, KiloCode and why CLI > MCP"](https://luketucker.com/openclaw-kilocode-and-why-cli-mcp/)
3. Medium/RentierDigital: ["Why CLIs Beat MCP for AI Agents"](https://medium.com/@rentierdigital/why-clis-beat-mcp-for-ai-agents-and-how-to-build-your-own-cli-army-6c27b0aec969)
4. LumaDock: ["OpenClaw skills guide"](https://lumadock.com/tutorials/openclaw-skills-guide)
5. WenHao Yu: ["25 Tools + 53 Skills Explained"](https://yu-wenhao.com/en/blog/openclaw-tools-skills-tutorial/)
6. LimitedEditionJonathan: ["Stop. Do Not Touch OpenClaw Until You Read This."](https://limitededitionjonathan.substack.com/p/writing-openclaw-skills-lej-guide)
7. DeepWiki: [Tools and Skills architecture](https://deepwiki.com/openclaw/openclaw/6-tools-and-skills)
8. Reddit r/LocalLLaMA: ["Do you still use MCP? Or use SKILLS?"](https://www.reddit.com/r/LocalLLaMA/comments/1r3p394/)

---

## OpenClaw's Three-Layer Extensibility

### Layer 1: Built-in Tools (25 total)
The actual capabilities — `read`, `write`, `edit`, `exec`, `process`, `web_search`, `web_fetch`, `browser`, `canvas`, `image`, `memory_search`, `message`, `cron`, `nodes`, `sessions_*`, etc.

Tools are the "organs" — without them, nothing happens. Controlled via `tools.allow` / `tools.deny` in `openclaw.json`.

### Layer 2: Skills (SKILL.md files)
Skills are "textbooks" — markdown files teaching the agent *how to use tools* for specific tasks.

**Progressive disclosure:** At session start, only name + description (~30-50 tokens per skill) loaded. Full SKILL.md read only when relevant. Deep reference files load on demand.

Skills load from three locations (highest to lowest precedence):
- `<workspace>/skills/` (per-agent)
- `~/.openclaw/skills/` (shared across agents)
- Bundled skills (53 official ones shipped with OpenClaw)

Gated at load time via metadata: `requires.bins`, `requires.env`, `requires.config`, `os` constraints.

### Layer 3: TOOLS.md (workspace file)
Freeform markdown — "user guidance for how to use external tools." Personal notes about what's installed. Does NOT control tool availability. Loaded into system prompt every session.

**Mental model (LimitedEditionJonathan):** "TOOLS.md is 'what's plugged in,' skills are 'how to use what's plugged in,' and cron prompts are 'what to do right now.'"

### Layer 4: Plugins (TypeScript extensions)
Full code modules running in-process. Can register RPC methods, HTTP handlers, agent tools, CLI commands, background services.

### Layer 5: ClawHub
Public skills registry at clawhub.com (3,000+ third-party skills). **Security warning:** 341 malicious skills found out of 2,857 audited (Feb 2026).

---

## Peter Steinberger's Argument: "CLI > MCP"

> "MCP is a crutch. The best thing that came out of MCP is it made companies rethink to open up more APIs."

Key points:
- **Context window bloat:** MCP pre-exports all functions + explanations. A weather service returns 50 fields when you only need "is it raining?" CLIs let you `| grep` for exactly what you need.
- **No chaining:** MCP calls are individual. Can't pipe or script. CLIs compose naturally via bash.
- **Fragile dependencies:** MCP servers are long-running processes that crash, need restarts, eat memory.
- **JSON precision:** MCP requires precise JSON blobs. Models are already great at bash.

---

## When MCP Makes Sense (from Reddit/community)

- When you need **live data from a running service** (databases, real-time APIs)
- When the tool provider only offers an MCP server (no CLI)
- When you want **structured tool schemas** with validation
- When you need a **shared tool layer** across multiple agent runtimes
- When running >1-2 MCP servers: a "tool gateway" helps with secrets + audit

---

## When CLIs Win

- Static data retrieval and transformation
- Anything benefiting from Unix pipes (`grep`, `jq`, `awk`)
- Custom tooling (faster to build CLI than MCP server)
- Token efficiency (filter output before it hits context)
- Composability (chain operations in single script)
- No daemon management overhead

---

## The Tradeoff Matrix

| Approach | Best For | Complexity | Native? |
|----------|----------|------------|---------|
| **Native tools** | Core ops (exec, browser, search) | Zero setup | ✅ Built-in |
| **Skills** | Teaching agent to use CLIs, workflows | Low (markdown) | ✅ Built-in |
| **Plugins** | Custom tools, channels, deep integration | Medium (TypeScript) | ✅ Built-in |
| **MCP (via plugin)** | External MCP servers | Medium | ❌ Community |
| **MCP (as server)** | Exposing OpenClaw to Claude/clients | Medium | ❌ Community |

---

## Key Quotes for Article

- Peter Steinberger: "MCP is a crutch."
- DHH: "If I was going to skate to where the puck is going... agents, like self-driving cars, don't need special equipment, like LIDAR or MCPs."
- LimitedEditionJonathan: "A skillless OpenClaw agent is just a REALLY expensive chatbot that forgets everything between sessions."
- WenHao Yu: "Tools are organs — they determine whether OpenClaw CAN do something. Skills are textbooks — they teach OpenClaw HOW."
- LumaDock: "Skills are not 'content' — they are an execution surface."
- RentierDigital: "Peter Steinberger built ~10 custom CLIs for OpenClaw, got recruited by OpenAI for it."
