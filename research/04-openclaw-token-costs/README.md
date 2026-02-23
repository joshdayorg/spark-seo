# Research Dossier #4: OpenClaw Token Costs

## Research Scope
5 sub-agents deployed: web cost overview, web optimization/pricing, X complaints, X tips, X real dollar amounts.

**Date:** February 22, 2026

---

## Executive Summary

OpenClaw is free. The AI brain is what costs money. This is the single most important framing for any token cost article. Monthly costs range from literally $0 (free-tier hosting + free-tier models) to $3,600+ (Opus on full blast). The median power user appears to spend $100-300/month. The conversation was ignited this week by Jason Calacanis on the All-In Pod saying his company hit **$300/day per agent** at just 10-20% capacity — that clip hit 2.68M impressions.

---

## The Cost Spectrum (Real Numbers)

### Individual Users:
| Spend Level | Source | Context |
|---|---|---|
| $0/mo | WenHao Yu guide | Oracle free tier + Gemini Flash-Lite |
| $3/mo | WenHao Yu guide | Oracle + Claude Haiku |
| $8/mo | WenHao Yu guide | Hetzner + GPT-4.1-mini |
| $8.80/mo | Daily Dose of DS | 3 agents 24/7 on Minimax M2.5 |
| $15/mo | @gc5388 | Rebuilt after OAuth crackdown |
| $20 in 2 hours | @Abirmajum | First-time user, no optimization |
| $35/mo | APIYI optimization case | After 77% cost reduction (was $150) |
| $50/mo | WenHao Yu guide | Hetzner + Claude Sonnet (premium) |
| $100/day | @Nek__12 | Opus as full-time assistant |
| $200/mo | @zxfrostbyte, @MysterE___ | Claude Max subscription (flat rate) |
| ~$670/mo (¥100K) | @ForexDog1968 | "Broke my spirit," switched to ChatGPT |
| $1,000/week | @antmillionsbot | Week 1 on Claude API |
| $1,250 in 3 weeks | @HelloBenWhite | Building an agent product |
| $3,600/mo | APIYI/Viticci | 1.8M tokens, extreme usage |

### Business/Production:
| Spend | Source | Context |
|---|---|---|
| $300/day per agent | Jason Calacanis (All-In Pod) | 10-20% capacity, scales to $100K/year |
| $4,200/week | @agi_dispatch | Contract analyzer, 500 docs in prod |
| $84,000 | @fluffykitten445 (satire) | 200+ Mac Minis in a closet |
| $21.4M | @benrzy | 40,000 Mac Minis, datacenter lease |

---

## Why OpenClaw Burns Tokens (Ranked by Cost Impact)

1. **Context accumulation (40-50% of cost)** — Entire conversation history resent every API call. One session occupied 56-58% of 400K window. This is the #1 cost driver.
2. **Tool output storage (20-30%)** — Large JSON responses, file contents, command outputs stored in history.
3. **System prompt overhead (10-15%)** — AGENTS.md, SOUL.md, etc. = 5,000-10,000 tokens per call.
4. **Multi-round reasoning (10-15%)** — Complex tasks need 5-10 API calls per user request.
5. **Model selection (5-10%)** — Opus at $5/$25 is 20x more expensive than Haiku at $0.25/$1.25.
6. **Cache misses (5-10%)** — 5-minute TTL means idle periods = full-price re-reads.

**Horror stories:**
- $200 burned in one day from infinite-loop automated task
- $50/day from heartbeat checks set to every 5 minutes
- "Broke my spirit" after ¥100K/month on Claude API

---

## Model Pricing Comparison (Feb 2026, per 1M tokens)

### Tier 1: Premium ($5-25 output)
| Model | Input | Output | Notes |
|---|---|---|---|
| Claude Opus 4.6 | $5.00 | $25.00 | Best quality, highest cost |
| Claude Opus 4.5 | $5.00 | $25.00 | Same tier |
| Claude Sonnet 4.6 | $3.00 | $15.00 | "Similar to Opus 4.5" — best value |
| GPT-5.2 | $1.75 | $14.00 | Cheaper input than Claude |

### Tier 2: Mid-Range ($1-10 output)
| Model | Input | Output | Notes |
|---|---|---|---|
| Gemini 2.5 Pro | $1.25 | $10.00 | Google's premium offering |
| GPT-5 | $1.25 | $10.00 | Same price as Gemini Pro |
| Claude Haiku 4.5 | $1.00 | $5.00 | Budget Claude with decent quality |

### Tier 3: Budget ($0.30-4 output)
| Model | Input | Output | Notes |
|---|---|---|---|
| Claude Haiku 3.5 | $0.80 | $4.00 | Solid budget option |
| Gemini 2.5 Flash | $0.30 | $2.50 | Outstanding value |
| GPT-5-mini | $0.25 | $2.00 | Budget GPT |
| GPT-4.1-mini | $0.40 | $1.60 | Battle-tested budget choice |

### Tier 4: Ultra-Budget ($0-0.42 output)
| Model | Input | Output | Notes |
|---|---|---|---|
| DeepSeek V3.2 | $0.28 | $0.42 | 10-50x cheaper than western |
| GPT-5-nano | $0.05 | $0.40 | OpenAI's cheapest |
| Gemini Flash-Lite | $0.075 | $0.30 | Absurdly cheap |
| Ollama (local) | $0.00 | $0.00 | Free, your hardware only |

---

## Optimization Strategies (Ranked by Community Signal)

### 1. Multi-Model Routing (19+ RTs)
Route different tasks to different models:
- Opus for complex reasoning
- Codex for coding
- Minimax/Qwen for research & creative
- Haiku/Flash for simple lookups

Viral config: "Brain is Opus, Codex writes code, Minimax 2.5 for research, Qwen 3.5 for creative" — RT'd 19 times.

### 2. Subscription Flat-Rate (29 RTs)
Claude Max at $200/mo reportedly delivers $2K+ equivalent API value. But:
- Anthropic reportedly cracking down on OAuth via 3rd-party tools
- Google actively banning Gemini OAuth users routing through OpenClaw
- Gray area: "Does Anthropic support auth via subscription?" — unclear

### 3. Multi-Provider Failover (16 bookmarks)
"Claude for quality, Groq/Llama for speed, GPT-4 as backup. Cost me 47 extra lines of code but workflows didn't go down."

### 4. Context Management
- Reset sessions regularly
- Use compaction wisely
- Limit tool output sizes
- Avoid storing large JSON in context

### 5. Local Models via Ollama
Free, runs on your hardware. Mistral and Qwen recommended for tool-capable local inference. Growing but still niche for OpenClaw users.

### 6. Infrastructure Optimization
- Oracle Cloud free tier: $0 hosting
- Hetzner ARM VPS: ~$4/mo
- Don't need Mac hardware for OpenClaw itself

---

## Prompt Caching Deep Dive

### Anthropic:
- Cache write: 25% premium on input price
- Cache read (hit): 90% discount on input price
- TTL: 5 minutes (resets on each hit)
- System prompts of 5K-10K tokens benefit enormously from caching
- Heartbeat < 5 min: wastes money keeping cache alive
- Heartbeat > 5 min: cache expires, cold-start re-reads

### OpenAI:
- Cache hit: 50-75% discount (automatic, no write premium)
- Automatic for prompts > 1,024 tokens

### Google:
- Batch API: 50% discount (Gemini 2.5 Pro drops to $0.625/$5)
- Free tier: 500 RPD for Flash

---

## The OAuth / Subscription Crisis

This is a live, evolving situation:
- Google actively banning Gemini OAuth users who route through OpenClaw
- Anthropic's position unclear — docs may say "not allowed for third-party" but OAuth was re-added
- Users caught in the middle: paid for subscriptions, then got banned/locked out
- Net effect: pushing some users from $200/mo flat-rate → raw API billing (much more expensive)
- Counter-trend: "Rebuilt my $200/mo setup for $15" using alternative providers

---

## Narrative Threads for Article

### Thread 1: "OpenClaw is free, Claude is not"
The fundamental misconception. OpenClaw costs $0. Your AI model provider charges by the token. This distinction matters for framing.

### Thread 2: "The $300/day agent problem"
Jason Calacanis's All-In Pod clip is THE conversation starter. 2.68M impressions. The rebuttal ("that's an engineering problem, not an AI problem") is equally important.

### Thread 3: "$0 to $3,600 — same tool, wildly different bills"
The range is absurd. Show the full spectrum from free-tier to Viticci's $3,600/mo and explain what drives the difference.

### Thread 4: "Context is the hidden cost"
40-50% of token spend is context accumulation. Most users don't realize how much they're paying to resend the same conversation history.

### Thread 5: "The $200/mo flat-rate vs pay-per-token war"
Claude Max users getting incredible value ($2K+ equivalent). But OAuth crackdowns threatening this model. Where does it end up?

### Thread 6: "The optimization opportunity"
$150/mo → $35/mo (77% reduction). Or even $200/mo → $15/mo. The tools exist, most people just don't use them.

---

## Sources

### Web Sources:
- https://help.apiyi.com/en/openclaw-token-cost-optimization-guide-en.html
- https://yu-wenhao.com/en/blog/2026-02-01-openclaw-deploy-cost-guide/
- https://blog.dailydoseofds.com/p/cut-openclaw-costs-by-95
- https://www.eesel.ai/blog/openclaw-ai-pricing
- https://platform.claude.com/docs/en/about-claude/pricing
- https://developers.openai.com/api/docs/pricing
- https://pricepertoken.com
- https://ollama.com/pricing
- https://simonwillison.net/2026/Feb/17/claude-sonnet-46/

### X Sources (by engagement):
- @theallinpod — $300/day per agent (2.68M impressions)
- @HedgieMarkets — All-In recap (276K impressions)
- @KanikaBK — 2.54B tokens tutorial (30.7K impressions)
- @hasan28d — "token costs burn" (22.1K impressions)
- @WorkflowWhisper — multi-provider failover (1,217 impressions)
- @HelloBenWhite — $1,250 in 3 weeks (1.3K impressions)
- @AlexFinn — provider crackdown backlash (507 impressions)

---

## Raw Research Files

1. `01-web-cost-overview.md` — Cost landscape, token burn analysis, deployment guides
2. `02-web-optimization.md` — Model pricing tables, caching economics, optimization strategies
3. `03-x-complaints.md` — Pain points, OAuth drama, cost awareness on X
4. `04-x-tips.md` — Community cost-saving strategies and model routing tips
5. `05-x-real-numbers.md` — Real dollar amounts from individuals and businesses
