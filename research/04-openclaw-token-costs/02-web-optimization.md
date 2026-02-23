# Web Research: Model Pricing & Cost Optimization Strategies

## Current Model Pricing (February 2026)

### Anthropic Claude

**Source:** https://platform.claude.com/docs/en/about-claude/pricing

| Model | Input (per 1M) | Output (per 1M) |
|-------|----------------|------------------|
| Claude Opus 4.6 | $5.00 | $25.00 |
| Claude Opus 4.5 | $5.00 | $25.00 |
| Claude Opus 4.1 | $15.00 | $75.00 |
| Claude Opus 4 | $15.00 | $75.00 |
| Claude Sonnet 4.6 | $3.00 | $15.00 |
| Claude Sonnet 4.5 | $3.00 | $15.00 |
| Claude Sonnet 4 | $3.00 | $15.00 |
| Claude Haiku 4.5 | $1.00 | $5.00 |
| Claude Haiku 3.5 | $0.80 | $4.00 |
| Claude Haiku 3 | $0.25 | $1.25 |

**Key insight:** Opus 4.6 and 4.5 now at $5/$25 — same tier as older models but the old Opus 4/4.1 at $15/$75 are 3x more expensive. **Sonnet 4.6** (released Feb 17, 2026) offers "similar performance to Opus 4.5" at Sonnet pricing ($3/$15) — best value in the Anthropic lineup.

### OpenAI

**Source:** https://developers.openai.com/api/docs/pricing

| Model | Input | Cached Input | Output |
|-------|-------|--------------|--------|
| GPT-5.2 | $1.75 | $0.175 | $14.00 |
| GPT-5.1 | $1.25 | $0.125 | $10.00 |
| GPT-5 | $1.25 | $0.125 | $10.00 |
| GPT-5-mini | $0.25 | $0.025 | $2.00 |
| GPT-5-nano | $0.05 | $0.005 | $0.40 |
| GPT-4.1 | $2.00 | $0.50 | $8.00 |
| GPT-4.1-mini | $0.40 | $0.10 | $1.60 |
| GPT-4.1-nano | $0.10 | $0.025 | $0.40 |
| o3 | $2.00 | $0.50 | $8.00 |
| o4-mini | $1.10 | $0.275 | $4.40 |

**Key insight:** GPT-5 at $1.25/$10 cheaper on input than Claude Sonnet ($3/$15). GPT-5-mini ($0.25/$2) and GPT-5-nano ($0.05/$0.40) are extremely budget-friendly. OpenAI cached input is 10% of base.

### Google Gemini

**Sources:** pricepertoken.com, costgoat.com, metacto.com

| Model | Input (<200K) | Input (>200K) | Output |
|-------|---------------|---------------|--------|
| Gemini 2.5 Pro | $1.25 | $2.50 | $10.00 |
| Gemini 2.5 Flash | $0.30 | — | $2.50 |
| Gemini Flash-Lite | $0.075 | — | $0.30 |

**Key insight:** Gemini 2.5 Flash at $0.30/$2.50 is outstanding mid-tier. Flash-Lite at $0.075/$0.30 is absurdly cheap. Google offers generous free tier (500 RPD for Flash). Batch API gives 50% discount.

### DeepSeek

**Source:** https://intuitionlabs.ai/articles/llm-api-pricing-comparison-2025

| Model | Input (miss) | Input (hit) | Output |
|-------|-------------|-------------|--------|
| DeepSeek V3.2-Exp | $0.28 | $0.028 | $0.42 |

**Key insight:** DeepSeek is the budget king — 10-50x cheaper than western providers. Quality may not match top-tier for all tasks but astounding for cost-sensitive workloads.

### Local Models (Ollama)

**Source:** https://ollama.com/pricing

- **Ollama local:** Completely free (open source, MIT). Run Llama 3.3, Qwen 2.5, Mistral on your hardware for $0/token.
- **Ollama Cloud Pro:** $20/mo
- **Ollama Cloud Max:** $100/mo
- Local hardware can be 2-15x cheaper per 1K tokens for steady workloads vs cloud APIs.

---

## Prompt Caching Economics

### Anthropic Cache Pricing:
- **Cache write:** 25% premium on input price
- **Cache read (hit):** 90% discount on input price
- **TTL:** 5 minutes (resets on each hit)

### OpenAI Cache Pricing:
- **Cache hit:** 50-75% discount (automatic, no write premium)
- Automatic for prompts >1,024 tokens

### Impact on OpenClaw:
- System prompts (AGENTS.md, SOUL.md, etc.) = 5,000-10,000 tokens resent every call
- If cache hits: ~$0.50/1M tokens instead of $5/1M (for Opus)
- If cache misses (>5 min idle): full price every time
- **Heartbeat intervals < 5 min waste money** by keeping cache alive unnecessarily
- **Heartbeat intervals > 5 min** let cache expire, causing cold-start re-reads

---

## Optimization Strategies (from sources)

### 1. Model Routing
- Use Opus/Sonnet for complex reasoning
- Use Haiku/Flash for simple tasks (summaries, lookups)
- Use local models (Ollama) for repetitive/low-stakes work
- Example: "Brain is Opus, Codex for coding, Minimax for research, Qwen for creative" (viral X config)

### 2. Context Management
- Reset sessions regularly (don't let context accumulate)
- Use compaction wisely (summarize instead of carrying full history)
- Avoid large tool outputs in context (trim JSON, limit file reads)

### 3. Subscription vs API
- Claude Max ($200/mo) reportedly delivers $2K+ equivalent API value
- But: Anthropic reportedly cracking down on OAuth usage through 3rd-party tools
- Google banning Gemini OAuth users routing through OpenClaw

### 4. Infrastructure
- Oracle Cloud free tier: $0 hosting
- Hetzner: ~$4/mo for ARM VPS
- Don't need Mac hardware for OpenClaw itself (just Node.js)

---

## Sources

- https://platform.claude.com/docs/en/about-claude/pricing
- https://developers.openai.com/api/docs/pricing
- https://pricepertoken.com
- https://costgoat.com/pricing/gemini-api
- https://intuitionlabs.ai/articles/llm-api-pricing-comparison-2025
- https://ollama.com/pricing
- https://simonwillison.net/2026/Feb/17/claude-sonnet-46/
- https://docs.openclaw.ai/concepts/compaction
