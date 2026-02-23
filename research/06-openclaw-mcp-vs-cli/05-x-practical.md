# X (Twitter) Research: Practical MCP + OpenClaw Experiences

---

## THE CLI vs MCP DEBATE IN PRACTICE

### @Th3RealSocrates — CLI purist, 50+ agents, zero MCP
> "The exec tool paired with on-demand skills is weirdly elegant. Running 50+ agents this way for 2 months. Zero MCP servers. Every skill is just a script that knows how to call an API or pipe through a CLI."
- 1 like, 233 impressions
- **Key insight:** Real practitioner with production experience. Skills = scripts wrapping APIs/CLIs.

### @freakingship — Subagent sessions > MCP context
> "@openclaw gemini 3.1 with thread-bound subagent sessions is actually sick. most mcp servers still can't hold context across calls"
- 1 like, 202 impressions
- **Key insight:** OpenClaw subagents maintain context where MCP servers can't.

### @agnus_dei_k — Migrated FROM MCP to OpenClaw native
> (Translated) "I used to use RevenueCat MCP Skills and GAS for a Slack notification bot, but stopped and integrated into OpenClaw instead."
- 16 likes, 6 bookmarks, 839 impressions
- **Key insight:** Real migration story: MCP → native integration.

---

## MCP ADVOCATE COUNTER-ARGUMENT

### @hasan28d — "MCP Connectors are easier" (22K impressions)
> "Everyone's busy flexing OpenClaw. Terminal installs. API keys. Debug loops. Meanwhile? I'm closing clients using Claude + MCP Connectors + Gamma — no setup drama."
- 72 likes, 42 RTs, 22 bookmarks

### @Lupin_Ai_Coder — Same template (10.5K impressions)
> "OpenClaw is powerful. But if you don't know how to debug and do token moderation, it's a nightmare."
- 86 likes, 45 RTs

**Note:** Multiple accounts pushing identical MCP Connectors + Gamma workflow — appears to be coordinated marketing, not genuine user experiences.

---

## SPECIFIC MCP SERVERS THAT WORK WITH OPENCLAW

### @OrderlyNetwork — Orderly MCP (DeFi trading)
> "The Orderly MCP is LIVE! You can now build, trade, and manage a perp DEX using nothing but AI."
- 44 likes, 10 RTs, 13 bookmarks, 12.6K impressions
- **Setup:** "Claude Desktop → drop one config block. OpenClaw → paste the repo link into chat."
- First concrete MCP server explicitly documenting OpenClaw integration.

### @SolanaProx — Dual-path (Skill OR MCP)
> "Install the skill: clawdhub install solanaprox. Or add the MCP server: npx solanaprox-mcp"
- 1 like, 25 impressions
- **Key insight:** Shows both paths — OpenClaw skill vs traditional MCP server.

### @WPDevTeam — WordPress + OpenClaw
> "WordPress + OpenClaw = real automation. From no-code MCP setups to full REST API control."
- 5 likes, 2 bookmarks, 120 impressions

### @John_Capobianco — VyOS networking MCP concept
> "fork the repo, use Claude to help make a vyos MCP (tools) and skills. Connect to slack. Use local LLM with Ollama for testing."
- 32 impressions
- **Pattern:** Fork → build MCP + skills → connect messaging → test locally → deploy with paid LLM.

---

## TOOLS CHOOSING CLI OVER MCP

### @chrisparkX (X Dev Platform) — xurl 1.0.3
> "Excited to push xurl 1.0.3 — our official X API CLI tool optimized for agents. Action chaining. Reusable skills. Fully integrated with OpenClaw."
- **551 likes, 38 RTs, 582 bookmarks, 74.5K impressions**
- **Key insight:** X's own dev platform explicitly building CLI for OpenClaw, not MCP.

### @elliotarledge — x-cli (anti-MCP positioning)
> "Introducing x-cli! Use it in claude code, codex, openclaw. It's a cli tool, not a MCP..."
- 72+ RTs
- Explicitly positioned as CLI alternative to MCP.

---

## PAIN POINTS & GOTCHAS

1. **Too many MCP servers → context bloat** (50 tool definitions = wasted tokens before starting)
2. **MCP servers can't hold context across calls** (OpenClaw subagents do this better)
3. **Token costs scale with debugging loops**
4. **Genuine tutorials hard to find** — @TradingAloha: "Not someone shilling their skills or mcp but an actual good walkthrough"
5. **LM Studio incompatibility** — can't invoke MCP calls through it
6. **Agent self-rejection** — CoinGecko MCP offered to agent that preferred direct API calls

---

## THE "AHA MOMENT" PATTERNS

1. When users realize `exec` + on-demand skills can replace most MCP servers with zero overhead
2. When subagent sessions maintain context where MCP servers can't
3. When tool developers (X/xurl, x-cli) choose CLI format over MCP — validates direction
4. When agents themselves prefer direct API over MCP (CoinGecko case)

---

## ADJACENT INNOVATIONS

### @lcgf__ — Claude Code broker (multi-agent MCP)
> (Portuguese) "I made my Claude Codes talk to each other. One finishes the API, sends the schema to the other to build the frontend. Lightweight broker, two files, zero config."
- MCP as inter-agent communication layer.

### @shao__meng — "Code in MCP" pattern
> "Code as tool: 'code in MCP' is more effective than 'MCP in code'"
- Citing @mitsuhiko's experiments. Influential in Chinese AI community.

---

## SUMMARY: THE PRACTICAL REALITY

**What practitioners actually do:**
- Power users (50+ agents): CLI/Skills, zero MCP
- Crypto/DeFi users: MCP for chain integrations
- Enterprise: MCP for internal APIs that LLMs haven't seen
- Most people: Haven't figured out the difference yet

**Industry momentum:** Tool developers (X Dev Platform, individual builders) explicitly choosing CLI over MCP for agent compatibility. The trend is toward CLI-native tools.
