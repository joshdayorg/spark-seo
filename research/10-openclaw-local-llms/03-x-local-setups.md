# OpenClaw + Local Models: Real Setups — X/Twitter Research

## HIGH-ENGAGEMENT ORIGINALS

### 1. @AlicanKiraz0 — 48-Mac OpenClaw Cluster (110 RTs)
> "My new 48x Openclaw & Local LLM Inference Cluster 🔥 - 1x M3 Ultra Mac Studio 512 GB Unified Ram - 4x M4 Mac Mini 16 GB -…"
- **110 RTs** — highest engagement in the dataset
- Aspirational hardware setup: massive Apple Silicon cluster for local inference

### 2. @ziwenxu_ — "10 Things I Wish I Knew Before OpenClaw" (38 RTs)
> "10 Things I Wish I Knew Before OpenClaw — 1. Localize heartbeats: Run Ollama like (llama3.2:1b) locally for heartbeat checks.…"
- **38 RTs** — practical tip: tiny local models for heartbeat/health checks to save API costs

### 3. @miaaowing — Pre-OpenClaw local setup nostalgia (24 RTs)
> "'openclaw' this 'openclaw' that — in 2023 i was running ollama 7B on my GTX 1080 hooked up to a discord bot"
- Community veteran: local agents existed before OpenClaw

### 4. @TheTuringPost — "20 Awesome Github repos" (12 RTs)
> "20 Awesome Github repos to build your own OpenClaw-style agents — Local model runners ▪️ Ollama ▪️ vLLM — Vector databases…"
- Curated list positioning Ollama/vLLM as go-to local runners

### 5. @JulianGoldieSEO — "Build AI agents for $0" (11 RTs)
> "Build AI agents for $0. No more API costs. No more manual work. Here's the new play 👇 → Link Qwen 3.5 with OpenClaw.…"
- The zero-cost agent pitch: Qwen 3.5 + OpenClaw + Ollama

### 6. @TheCryptoLif7 — "Every local system breaks in production" (6 RTs)
> "Every local AI multi-agent system you've seen demoed this year breaks in production. Every. Single. One."
- Skepticism/frustration — local multi-agent setups don't survive production

---

## TOOL CALLING & MCP ISSUES

### @BeforeAGI13 — LM Studio MCP Broken
> "Another limitation I found is. You cannot make an API request with LM Studio and invoke MCP calls through it."
- 193 impressions | LM Studio + MCP integration broken via OpenWebUI pipeline

### @_overment (Adam) — GLM 4.7 Flash Works
> "of course you can, since MCP tools are all about exposing tool schemas for function calling. below, we have Gmail MCP with GLM 4.7 Flash through LM Studio, running locally on my MacBook."
- GLM 4.7 Flash works with MCP through LM Studio **directly** (not via OpenWebUI)

### @BeforeAGI13 — OpenWebUI Strips MCP Calls
> "Yeah it feels like openWebUI is sanitizing the API call and removing the MCP portion... ended up just going raw LM Studio."
- Workaround: use LM Studio directly, bypass OpenWebUI

---

## MODEL EXPERIENCES

### Models That Frustrate
**@ArnavGoyal_X** — Qwen 2.5 7B:
> "I mean setting up its brain as ollama qwen 2.5 7b is uggghhhh."

**@SteveMojica** — General local LLMs:
> "Local LLMs with OpenClaw? It's like teaching a toddler calculus they can barely form sentences but you expect complex outputs."

### Models That Work
**@dukunroot** — Mistral/Qwen recommended:
> "currently it support self hosted LLM like ollama just used the LLM which have tool capabilities, recommend to use Mistral/Qwen"

**@_overment** — GLM 4.7 Flash confirmed:
- Successfully running Gmail MCP locally on MacBook via LM Studio

### Context Window Limits
**@axiomofmind** — Raspberry Pi can't handle it:
> "You can run a small model on the pi like mistral3b via ollama... but trying to run openclaw would not work because the context needs to be larger than what can fit on the pi."

---

## HARDWARE PEOPLE USE

| Hardware | Who | Use Case |
|----------|-----|----------|
| M3 Ultra Mac Studio (512GB) | @AlicanKiraz0 | 48-node cluster lead |
| Mac Mini M4 (16GB) | @AlicanKiraz0 | Cluster nodes (4x) |
| MacBook (unspecified) | @_overment | LM Studio + GLM 4.7 Flash |
| GTX 1080 | @miaaowing | Nostalgic: Ollama 7B pre-OpenClaw |
| H100 GPU | @ozgurozkan123 | Multi-tenant pentesting ($200/user/mo) |
| RTX 5070 Ti (16GB) | Reddit user | Qwen3-8B, "sorta functional" |
| Raspberry Pi | @axiomofmind | Confirmed NOT enough for OpenClaw |

---

## COST SIGNALS

**@grok** — explaining Mac Mini appeal:
> "Running it on a Mac Mini/Studio doesn't change cloud API fees if you stick with remote models. But yes—switch to local Ollama models on the Mac and you eliminate recurring API costs entirely. Upfront hardware investment, zero token burn after."
- 184 impressions, 2 bookmarks

**@ozgurozkan123** — H100 pricing:
> "multi-instance openclaw but for pentesting... I need 39 more tenants (39 customers) to cover the h100 costs with $200 per month per user"
- Enterprise model: shared H100, $200/user/month across 40 users

---

## HIDDEN GOTCHA

**@DJohnstonEC** — Local embeddings not default:
> "Most OpenClaw users probably have no idea their memory_search does nothing. The local embedding option exists (node-llama-cpp ships with OpenClaw), but it's not the default. You have to explicitly set provider: 'local'."
- 75 impressions | node-llama-cpp ships with OpenClaw but users don't know to enable it

---

## KEY PATTERNS

1. **Quality gap is real** — "teaching a toddler calculus" is the honest take on small local models
2. **Tool calling is broken** for most local models — Qwen 2.5 Coder returns JSON text instead of calling tools
3. **GLM 4.7 Flash** is the confirmed working local model for MCP/tool calling
4. **Hybrid is the answer** — local for heartbeats/data gathering, cloud for reasoning
5. **Raspberry Pi explicitly NOT enough** for OpenClaw + local inference
6. **48-node Apple Silicon clusters** are being built (aspirational but real)
7. **Hidden gotcha**: local embeddings exist but aren't enabled by default
