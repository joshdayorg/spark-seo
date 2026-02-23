# Local/Small Models for Agent Tasks: Routing & Quality — X/Twitter Research

## THE HYBRID ROUTING PATTERN (Dominant Consensus)

The clearest signal: **model routing by task complexity** is the community's answer to cost vs quality.

**@HappyGezim** (Gezim, lobsterfarm.ai) — 47 impressions:
> "depends entirely on model routing tbh. if you run everything through opus you'll burn $20+/day easy. the trick: route 90% of tasks through cheaper models (kimi k2.5, gemini flash) via openrouter - same quality for browsing, emails, basic stuff at like 1/50th the cost. save opus"

**@TOBIETOM** (TobiasBond, PineOT):
> "The 'sweet spot' is model routing: using Haiku for the simple logic and only [escalating to Opus for complex reasoning]"

**@AlanKLFeng** (Alan Feng) — 24 impressions:
> "Per-channel model routing is one of OpenClaw's most underutilized features. I do something similar — local model for casual chat, Claude Sonnet for code, and Opus only when I need deep reasoning. Saves a ton without sacrificing quality where it matters"

**@agent_republic** — 19 impressions:
> "Use model routing by task instead of one default model for everything."

**Chinese-language tweet (19 RTs):** OpenClaw guide using Opus as "brain," Codex for code, MiniMax 2.5 for research, Qwen 3.5 for creative writing.

---

## LOCAL MODEL QUALITY COMPLAINTS (Honest Takes)

**@ArnavGoyal_X** — Qwen 2.5 7B:
> "setting up its brain as ollama qwen 2.5 7b is uggghhhh."

**@ClaudiusMaxx** — 274 impressions, 2 likes:
> "ran GLM for 2 weeks, same experience — infrastructure was solid but every draft read like a linkedin post from 2019. switched creative work to Claude, kept the cheap model for ops/cron/plumbing. night and day difference"

**@CodeByAaron** — 18 impressions:
> "The draft problem is 100% a model problem. Switched content crons to Sonnet and the quality gap was immediate. Keep the cheap model for infrastructure, use the better one where quality matters."

**@itsPaulAi** — 243 impressions:
> "People aren't running the LLM on the Mac Mini for OpenClaw no. It'll be completely useless without a large model, you need a very expensive Mac Studio minimum."

---

## THE $0 STACK: Qwen 3.5 + Ollama + OpenClaw (Viral)

**@JulianGoldieSEO** — **73 RTs, 417 likes, 703 bookmarks, 25,976 impressions**
- Setup: Ollama → pull qwen3.5-cloud → point OpenClaw at Ollama endpoint
- Claims "free within token limits" (weekly reset)
- Follow-up: 266 likes, 374 bookmarks, 21,127 impressions

---

## THE ZIWEN BLUEPRINT (Most Cited Practical Setup)

**@ziwenxu_** — **521 likes, 820 bookmarks, 56,951 impressions** (heartbeat tweet):
> "Stop paying for OpenClaw heartbeats. Got a 16GB Mac Mini? Run a local LLM for heartbeats and save tokens within 10 seconds."

**@ziwenxu_** — **100 likes, 211 bookmarks, 23,699 impressions** (full blueprint):
> "Here's how I run a 24/7 AI workforce on M4 Mac Mini for $50/mo. After 9-agent team burned $100 overnight on useless chatter, I ditched costly cloud APIs. Switched to hybrid: MiniMax + local LLM on Doraemon (M4 Mini). Now run 4 autonomous agents on hourly [crons]"

Architecture detail:
> "Minimax for both Claude Code + OpenClaw. Local LLM for [heartbeats] to avoid hitting rate limits too hard."

"10 Things I Wish I Knew" (38+ RTs):
> "Localize heartbeats: Run Ollama like (llama3.2:1b) locally for heartbeat checks."

---

## MODEL RECOMMENDATIONS TABLE

| Model | Use Case | Who Recommends | Notes |
|-------|----------|----------------|-------|
| **Qwen 3.5 (Ollama)** | Full local agent | @JulianGoldieSEO | "$0 stack" viral pitch |
| **Qwen 2.5 7B** | Attempted as brain | @ArnavGoyal_X | Frustrating — too small |
| **Mistral/Qwen** | Self-hosted + tools | @dukunroot | Tool-calling capable |
| **Llama 3.2 1B** | Heartbeat only | @ziwenxu_ | Tiny, just for pings |
| **MiniMax 2.5** | Mid-tier cloud alt | @ziwenxu_, Chinese tweet | Research tasks |
| **Gemini 2.5 Flash-Lite** | Budget cloud | @OpenClawTips | $0.10/1M in, $0.40/1M out |
| **Kimi K2.5** | Budget via OpenRouter | @HappyGezim | 1/50th Opus cost |
| **GLM** | Tried for content | @ClaudiusMaxx | "LinkedIn post from 2019" |
| **Claude Sonnet** | Code, quality content | @AlanKLFeng | The quality bar |
| **Claude Opus** | Deep reasoning only | @HappyGezim | Reserved for complex tasks |
| **Haiku** | Simple logic | @TOBIETOM | Cheap routing tier |

---

## COST DATA POINTS

- **$100/night** burned by 9-agent team on cloud (before optimization) — @ziwenxu_
- **$50/mo** after hybrid local+MiniMax — @ziwenxu_
- **$20+/day** everything through Opus — @HappyGezim
- **$0** Qwen 3.5 + Ollama (free tier limits) — @JulianGoldieSEO
- **90% cost reduction** via model routing — @HappyGezim
- **Gemini 2.5 Flash-Lite**: $0.10/1M in, $0.40/1M out — @OpenClawTips
- **1/50th cost** routing simple tasks to cheap models — @HappyGezim

---

## QUALITY THRESHOLD CONSENSUS

Community hierarchy:
1. **Local small (7B-13B)**: Good for heartbeats, cron plumbing, basic ops — **NOT for content, creative, complex reasoning**
2. **Budget cloud (MiniMax, Kimi, Gemini Flash)**: "Same quality for browsing, emails, basic stuff" — good for 90% of tasks
3. **Premium cloud (Sonnet)**: Required for code and quality-sensitive content
4. **Top-tier (Opus)**: Reserved for deep reasoning only

**Key insight:** Nobody recommends local small model as the *only* brain. Universal pattern is tiered routing.

---

## KEY PATTERNS

1. **Hybrid routing is the answer** — local/cheap for 70-90% of tasks, cloud for 10-30%
2. **Per-channel routing** is an underused OpenClaw feature
3. **Content quality has a hard floor** — GLM/small models produce "LinkedIn post" quality
4. **llama3.2:1b for heartbeats** is the most specific, actionable tip (from @ziwenxu_)
5. **$50/mo** is the realistic cost for a well-optimized 24/7 agent setup
6. **Nobody runs local-only successfully** — every real user has cloud fallbacks
