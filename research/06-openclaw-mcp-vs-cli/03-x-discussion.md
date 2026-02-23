# X (Twitter) Research: MCP + OpenClaw Discussion

## 🔥 Viral Thread: @chrysb — "MCP is dead in the water"

**Date:** Feb 22, 2026 | **296 likes, 65 replies, 16 RTs, 12 quotes, 213 bookmarks, 25,244 impressions**

> "unpopular (maybe?) opinion: MCP is dead in the water. @openclaw has shown me that api & cli will win. every MCP server you connect loads its tool definitions into your context window. name, description, parameter schema, all of it. connect 10 servers with 5 tools each and..."

THE tweet of the moment. Massive engagement for a technical opinion. The thread is a genuine, substantive debate.

---

## Team "CLI/Skills Win Over MCP"

| User | Quote | Impressions |
|------|-------|-------------|
| @chrysb | "MCP is dead in the water" (thread starter) | 25,244 |
| @franjohn21 | "mcp is dead in the water. it was dead at takeoff... API + cli is a big simplicity improvement" | 307 |
| @vlelyavin | **"mcp is docker compose for people who forgot curl exists"** | 73 |
| @pixelandpump | "mcp is what happens when you let enterprise architects design for agents. cli is what happens when you let hackers automate their own jobs" | 56 |
| @chrisbmullins | "With Skills we don't need MCP any more" | 19 |
| @joshgonsalves_ | "I always preferred native tools and now Skills to building out MCP servers" | 71 |
| @kissapiai | "we went from 'just curl it' to enterprise middleware for AI lol" | 6 |
| @itsEvKlev | "MCP and ACP are arguing about who gets to be the standard while a Rust binary on stdio already does the job" | 6 |

---

## Team "MCP Has Its Place"

| User | Quote | Impressions |
|------|-------|-------------|
| @RobertClaypool | "MCP is a *stateful bidirectional channel*... server can PUSH notifications, maintains SESSION STATE" | 10 |
| @Bu5yGiraffe | "This argument assumes context windows continue to be a problem. Sonnet just showed a 1M context window" | 95 |
| @johnny_ver_2 | "How can MCP be dead if Google and Microsoft are creating WebMCP?" | 15 |
| @lonniev | "MCP gives a low-level CRUD REST API a semantic wrapper that communicates intention" | 84 |
| @PaulMaddison121 | "CLI is great until the model is calling production APIs that move money... someone's going to want an audit" | 24 |
| @Michaelzsguo | "in enterprise, our internal APIs have never been seen by any public LLM" | 91 |

---

## Nuanced Middle Ground

| User | Quote | Impressions |
|------|-------|-------------|
| @0xNomad_ | "Shifting to a skill just means an API/CLI and a ton of skill instructions. You pay either way" | 132 |
| @amar_patel | "openclaw has tiny context windows and MCPs are overkill. However, MCPs have their place in enterprise" | 102 |
| @evadne | "exec tool is great for dynamic loading. One thing that never worked: telling the model to call a new tool not in initial schema" | 120 |
| @NiteLiteDF | "'exec' only executes what the model knows about. Tool definitions introduce new capabilities" | 28 |
| @lukaswolf_ | "Couldn't one have a one-liner per mcp server and agent 'explores' the docs similar to --help?" | 45 |
| @kyzookyzoo | "put the API docs in the repo, tell the agent to refer to them. Maybe have the agent write its own skill" | 8 |

---

## Real MCP Servers People Use With OpenClaw

| MCP Server | User | Use Case | Impressions |
|------------|------|----------|-------------|
| **Vercel** | @WebWrld8 | Deploy projects via MCP, never run locally | — |
| **Gmail + Google Calendar** | @talakoubali | "supercharged my agent... don't need gog skill" | — |
| **Amazon** | @meimakes | Agent adds items to cart via custom MCP | 716 |
| **Commune (email outreach)** | @shanjai_raj | "1200 emails sent across 5 inboxes" | — |
| **Supabase** | @_Daniel_Ospina | Migrations directly on supabase | — |
| **CoinGecko** | @TheoArt10 | Agent rejected MCP, preferred direct API | — |
| **Home Assistant** | @DMOGeek | Home automation via MCP | — |
| **RSSdeck-MCP** | @imFong | "turns RSS feeds into token-efficient intelligence" | — |
| **Pushary** | @aadilbuilds | Push notifications from AI agents | — |

**Notable:** CoinGecko MCP was offered to an agent that decided direct API calls were better — the agent itself rejected MCP.

---

## MCP Pain Points

1. **Context window bloat** — #1 complaint. 50+ tool schemas consuming tokens before real work.
2. **Stateful complexity** — long-running processes that crash, need restarts.
3. **Security risks** — 341 confirmed malicious skills (Snyk report).
4. **Enterprise mismatch** — internal APIs unseen by LLMs need definitions, but overhead is real.
5. **Agent self-rejection** — agents sometimes prefer direct API over MCP.

---

## Sentiment Split (~50 unique voices)

- **~55% lean CLI/Skills** — power users, hackers, indie builders
- **~25% lean MCP has its place** — enterprise, stateful connections, push notifications
- **~20% nuanced middle** — "both have costs, problem is temporary as context windows grow"

**Cultural divide:**
- Solo builders / OpenClaw power users → CLI/Skills
- Enterprise / multi-party workflows → MCP
- Crypto/DeFi → MCP (for chain integrations)
