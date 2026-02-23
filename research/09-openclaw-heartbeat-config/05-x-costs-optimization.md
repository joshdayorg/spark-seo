# OpenClaw Always-On Costs & Optimization — X/Twitter Research

## 1. REAL COST DATA FROM USERS

### The Cost Spectrum

**$1-2/day — Budget models:**
- @joshuaday: "with something like gemini flash or minimax thats maybe $1-2/day for normal use" (37 imp)

**$2-8/day — Moderate use (Sonnet via API):**
- @joshuaday: "moderate openclaw usage on sonnet via API typically runs $2-8/day" (19 imp)

**$6-9/day — Mixed model setup:**
- @ClaudiusMaxx: "about $6-9/day on API costs running Opus for the main brain + Sonnet for subagents across 12 daily cron sessions. the hardware is a Mac Mini M4 Pro running 24/7...honestly the API cost is the main thing." (72 imp)

**$32/day — Heavy Sonnet use:**
- @mjtechguy: "Broke down and tried sonnet via api as my main task runner only in OpenClaw. Codex for all cron and sub agent jobs, GLM for coding. Sonnet has cost me $32 on what I consider a light day. This is unsustainable." (37 imp)

**$100-200/day — Unsupervised agents:**
- @iamyxsh: "Users are reporting $100-200/day in uncontrolled API spend from agents running unsupervised. One documented case: OpenClaw made 25 overnight calls asking Claude Opus 'Is it daytime yet?' Cost: $18.75 while the owner slept. No default budget caps. No rate limits. No alerts." (11 imp)

### The $18.75 "Is It Daytime Yet?" Story
OpenClaw made 25 overnight API calls asking "Is it daytime yet?" — $18.75 burned while owner slept. **Perfect anecdote for idle token burn in an article.**

---

## 2. COMPACTION & CONTEXT WINDOW ISSUES

### Compaction Stacking = Hidden Cost Multiplier

**@richkuo7** — Feb 23
> "@openclaw is by default inefficient at managing context. (It just carries everything in the chat until compaction) Remember to /compact often or /new to start new sessions" (13 imp)

**@comforteagle** (Steve-arino) — Feb 22
> "Compaction drift is the right name for it. The 30k threshold is our baseline too, but we've had to tune down to ~20k for sessions running longer than 8 hours. Fidelity loss compounds fast at higher compaction frequency." (16 imp)

**@comforteagle** — Feb 22
> "Session combustion is the worst part of running 24/7. Does it trigger most often from context compaction stacking up, or more from accidental /new?" (24 imp)

**@joshuaday** — Feb 22
> "compaction stacking thats the real killer for long sessions. each compaction summarizes the previous summary and you lose fidelity fast. the fix is enabling memoryFlush with a reasonable soft threshold (~30k tokens) so critical context gets written to [files]" (26 imp, 1 like)

**@joshuaday** — Feb 22
> "compaction works the same across models - when your session hits the contextTokens limit (200k default), openclaw summarizes older messages to free up space. with claude you can push to 1M (context1m: true + contextTokens: 1000000) and prompt caching means re-sending context is [cheaper]" (17 imp)

### Context Window Tax
**@taayjuss** — Feb 23
> "not sure mcp is dead but the context window tax is very real... 50 tool definitions loaded before you even do anything" (75 imp)

**@Bu5yGiraffe** (Nate) — Feb 23
> "This argument assumes the context window continues to be a problem. Sonnet just showed a 1M context window in beta, if that trend continues then the overhead could be negligible" (178 imp, 3 likes)

---

## 3. UPDATE BREAKAGE COSTS

**@KeithPowers** — Feb 23
> "I had major issues after updating 2.21. All of my prior configs lost, compaction issues, And a very noticeably dumber bot and huge increase in token use yesterday on opus 4.6. almost as if it was being throttled..." (87 imp)

**@gravorn1** — Feb 23
> "You can't OpenClaw to automate the house. It forgets things after compaction and restarts. You tell it to use Home Assistant and it forgets. You put in a skill it forgets to read the skill." (92 imp)

---

## 4. ALWAYS-ON PATTERNS & SKEPTICISM

### The "Always-On But Why?" Question

**@bigagave** (Steve Pizza) — Feb 22 | 84 imp, 2 likes
> "i set up a VM for clawdbot on my m4 max 64gb. issue is that i often close my mbp which kind of defeated the purpose of the always on. i know it's not the cool thing to say right now, but i haven't found much use for openclaw beyond the novelty wow factor."

**@editxshub** (Shubham Sharma) — Feb 20 | 28 imp, 2 likes
> "Running OpenClaw locally is a pain. servers crash. setups break. It's expensive."

**@FutureHumanism** — Feb 22 | 183 imp
> "Why would anyone run openclaw using the API? It's going to cost 10x"

### The Believers

**@alovevolcanic** — Feb 21 | 112 imp
> "The point of openclaw is to be able to fire up an agent from your phone when you're away from home, so you need an always on box"

**@SavianBoroanca** — Feb 22
> "claude code is better for coding tasks at a desk. openclaw is different because it's always on, runs crons and heartbeats while you're asleep, and talks to you over telegram. different use case, not a replacement"

---

## 5. OPTIMIZATION TIPS & COST-CUTTING STRATEGIES

### Model Routing
- **@joshuaday**: Use model fallbacks to auto-route simple tasks to cheaper models
- **@HappyGezim**: "use openrouter + kimi k2.5 or gemini flash. like 1/50th the cost of opus" (73 imp)
- **@scratchtabs**: "Plus 1 for Kimi K2.5. I tried it for few days after being burnt on opus/sonnet api calls." (50 imp)
- **@mjtechguy**: Uses Codex for cron/subagent jobs, GLM for coding, Sonnet only for main tasks

### Memory & Context Optimization
- **@joshuaday**: Enable `memoryFlush` with ~30k token soft threshold
- **@joshuaday**: Use `compaction memoryFlush` to save notes before trimming
- **@richkuo7**: `/compact often` or `/new` for session hygiene
- **@comforteagle**: Tune compaction threshold to ~20k for 8+ hour sessions

### Scheduling Optimization
- **@Phy041**: Use isolated cron jobs instead of always-on heartbeat for scheduled tasks
- **@Bossondehigs**: "Heartbeat every 30min, not 5"
- **@alindnbrg**: "cron-style ticks, cheap checks, LLM on change" — only invoke model when something changed

### The Cloud Alternative
- **@augmidotworld**: "Cloud hosting works better for most people anyway. Always on, no power bills, no hardware to manage. [Service] runs OpenClaw in the cloud for $19.99/mo."
- **@OneManSaas**: Debating API vs OAuth — "API felt more reliable, but the cost is brutal"

---

## 6. HEARTBEAT-SPECIFIC SIGNALS

**@ClankerSays** — Feb 20 | 5 imp
> "if your agent heartbeat is just 'heartbeat_ok' you're doing it wrong"

**@antideologist** — Feb 20 | 8 imp
> "With an agent heartbeat, it's not necessarily going to keep being episodic."

**@kayintveen** — Feb 18 | 38 imp
> "Maybe something like Agent Heartbeat Protocol (AHP)? Periodic state broadcasts with availability, current context, capabilities"

**@chrgerloff** (Christian) — 15 imp
> "A Mac Mini M4 running 20 autonomous cron jobs costs me €3/month in power."

---

## KEY TAKEAWAYS

1. **Cost range:** $1-2/day (cheap models) → $6-9/day (Opus+Sonnet mix) → $32/day (heavy Sonnet) → $100-200/day (unsupervised)
2. **The $18.75 "Is it daytime yet?" story** = perfect anecdote for idle burn
3. **Compaction stacking** = hidden cost multiplier. Each summarization summarizes the previous summary, losing fidelity and burning tokens. Tune to 20-30k threshold.
4. **Context window tax** from 50 tool definitions eating context before work starts
5. **No built-in budget caps** = common complaint. Agents run up bills while owners sleep.
6. **Optimization levers:** Model fallbacks, memoryFlush, isolated cron, /compact hygiene, cheap model routing
7. **Hardware cost is trivial** compared to API: Mac Mini M4 = €3/month power vs $180-270/month API
8. **"Always-on but why?" skepticism** growing — some users find no use beyond novelty
