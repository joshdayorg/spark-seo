# X (Twitter) Research: OpenClaw Context Window Pain Points

## VIRAL THREADS

### @johann_sath — "Agent Gets Dumber" (577 likes, 1,556 bookmarks, 49K impressions)
> "most openclaw users hit 150K context in 1 conversation & wonder why their agent gets dumber. here's how to never hit the limit: add 'you are the orchestrator. subagents execute.' to AGENTS.md → main session stays lean, heavy work runs in fresh context windows"
- 39 RTs, actively spreading

### @chrysb (Chrys Bader) — "MCP is Dead / Context Bloat" (201 likes, 141 bookmarks, 16.8K impressions)
> "every MCP server you connect loads its tool definitions into your context window. name, description, parameter schema, all of it. connect 10 servers with 5 tools each and..."
- 51 replies, 8 RTs

### @chrysb — "Drift Visibility" (75 likes, 141 bookmarks, 5.7K impressions)
> "to avoid @openclaw drift, it's critical to have visibility into the changes it makes. add this to your AGENTS md prompt to make sure it always shows its work"

## DIRECT USER COMPLAINTS

### @gravorn1 — Forgets After Compaction
> "You can't OpenClaw to automate the house. It forgets things after compaction and restarts. You tell it to use Home Assistant and it forgets."

### @comforteagle — Compaction Drift Scaling
> "Compaction drift is the right name for it. The 30k threshold is our baseline too, but we've had to tune down to ~20k for sessions running longer than 8 hours. Fidelity loss compounds fast."

### @LeanLabsMD — Things Only Live in Context
> "all too often it has told me that it did something when that 'thing' only lived in the context window.... new session -> blank and I'm rebuilding again"

### @VinnyumTech — Hallucinated Actions
> "it has been telling it did things that it didn't actually do. He was supposed to be looking at some URLs and reporting some data, but yesterday it told me that it can't not get data because web search is not active but it reported data…"

### @RealLanceJones — Self-Surgery Disaster
> "Memory drift, token usage… please, for the love of all that is Holy, do NOT let your OC agents perform surgery on themselves."

### @MOOOOrion — Cost Per Message Shock
> "OpenClaw is the most context-bloated thing I've ever seen. It told me each message in a long conversation is $2."

### @chefamysins — Empty Promises on Smaller Models
> "so frustrated with empty promises, and honestly it is 100% model related. So many not completed projects on less complex models."

## COST COMPLAINTS

### @hasan28d (42 RTs, 22K impressions)
> "Everyone's busy flexing OpenClaw. Terminal installs. API keys. Debug loops. Watching token costs burn with every mistake."

### @platonicspace
> "openclaw is great but it costs money to run on an api basis with a good enough model. until the cost is figured..."

### @benoitsartre
> "OpenClaw is not easy to install. You have to manage cost LLMs, slim contexts, Openrouter, debuggings, etc."

## WORKAROUNDS SHARED

### @Flippin4Gold — Discord Channels as Context Isolation
> "Each channel you set up becomes its own session. If you stay vigilant to keep the conversation focused, your context bloat/drift is much less frequent"

### @kissapiai — The Core Problem
> "the context bloat thing is what kills me. 50 tool schemas eating tokens before you even ask a question??"

### @lukaswolf_ — Lazy Loading Suggestion
> "why are mcp servers so wasteful with context? Couldn't one have just a one liner per mcp server and then the agent 'explores' the mcp docs similar to -help via cli?"

## SUMMARY: Top Pain Points

1. Compaction drift / amnesia — agent forgets after compaction (5+ tweets)
2. Hallucinated task completion — says it did things it didn't (4+ tweets)
3. Context bloat from tools — tool schemas eat tokens before real work (6+ tweets)
4. Cost scales with context — $2/message in long conversations (5+ tweets)
5. No built-in visibility/audit — no way to see what actually happened (3+ tweets)
6. Agent self-modification fails — editing own memory causes cascading drift (2+ tweets)
7. Setup complexity barrier (3+ tweets)
