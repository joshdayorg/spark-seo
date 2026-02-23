# Ollama + OpenClaw Integration, Local AI Hosting & Sovereignty — Web Research

## Ollama Integration (First-Class, Official)

Ollama has a dedicated OpenClaw page. Single-command setup:

```bash
ollama launch openclaw
```

Auto-handles: installing OpenClaw (via npm), security notice, model selection (local/cloud), provider config, gateway daemon, TUI launch.

### Technical Details
- OpenClaw requires **minimum 64k token context window**
- Ollama exposes OpenAI-compatible API at `http://localhost:11434/v1`
- Config: `"api": "openai-completions"` with `"apiKey": "ollama-local"`
- Gateway auto-reloads when models change
- Config-only mode: `ollama launch openclaw --config`
- Specific model: `ollama launch openclaw --model kimi-k2.5:cloud`

### Recommended Models (Ollama Docs)
- **Local:** `glm-4.7-flash` (~25GB VRAM), `qwen3-coder`, `gpt-oss:20b`, `gpt-oss:120b`
- **Cloud (free via Ollama):** `kimi-k2.5` (1T parameter, multimodal), `minimax-m2.1`, `glm-4.7`

Sources: https://ollama.com/blog/openclaw, https://docs.ollama.com/integrations/openclaw

---

## Local Inference Servers Compatible with OpenClaw

All expose OpenAI-compatible APIs — plug-in replacements:

| Tool | Best For | Throughput | Notes |
|------|----------|-----------|-------|
| **Ollama** | Dev, single-user | ~41 TPS | 1-command setup |
| **vLLM** | Production, high concurrency | ~793 TPS (19x Ollama) | PagedAttention reduces memory fragmentation 40%+ |
| **LM Studio** | Desktop, GUI | Good single-user | Dual-engine: native MLX on Macs, llama.cpp elsewhere |
| **LocalAI** | Multimodal, API hub | Varies | Universal API hub — routes text to vLLM, images local |
| **llama.cpp** | Edge, CPU-only | ~80 TPS (CPU) | Manual setup, via wrappers |

**vLLM standout:** At 128 concurrent users, sub-100ms P99 latency vs Ollama's 673ms.
**LM Studio:** Often outperforms Ollama on integrated graphics via Vulkan offloading.

Source: https://blog.premai.io/self-hosted-llm-guide-setup-tools-cost-comparison-2026/

---

## Privacy & Data Sovereignty Arguments

### Core Arguments
- 44% of organizations cite data privacy as #1 barrier to LLM adoption (Kong 2025)
- Every prompt to OpenAI/Anthropic/Google touches external servers — dealbreaker for healthcare, finance, legal
- Self-hosting = no third-party retention, no training on inputs, no compliance gray areas

### Regulatory Drivers
- GDPR Art. 44 restricts cross-border transfers — local models eliminate headaches
- HIPAA §164.308 — offline model + local API key-gate solves access control
- Schrems II — no DPAs with U.S. cloud providers needed
- UK £500M Sovereign AI Unit (July 2025)

### Scale of the Shift
- 55% of enterprise AI inference now on-premises/edge (up from 12% in 2023)
- Local execution reduced average response from 1.5s to under 40ms for enterprise
- European/Asian government spending on nationalized AI infra grew 140% YoY

Sources: https://renewator.com/the-rise-of-local-llms-privacy-and-sovereignty-in-2026/, https://createaiagent.net/self-hosted-llm/

---

## Cost Comparisons: Local vs Cloud API

### Per-Token Costs
| Approach | Cost per 1M tokens | Monthly (heavy use) |
|----------|-------------------|---------------------|
| GPT-4o API | $2.50 in / $10.00 out | $300–$1,500/mo |
| GPT-4o Mini API | $0.15 in / $0.60 out | $150–$600/mo |
| DeepSeek API | ~$0.14 | Very cheap but privacy risk |
| Self-hosted 7B (H100 spot) | ~$0.013/1K tokens | ~$10.3K/yr all-in |
| Ollama local (personal) | $0 (electricity) | ~$30-60/mo electricity |

### Enterprise Case Studies (Ptolemay)
- API bills escalated $15K → $35K → $60K/month within 3 months at 1.2M messages/day
- Tele-medicine: $48K → $32K/month by moving chat triage to self-hosted
- SaaS platform: $42K → $29K/month routing 70% to smaller model

### Break-Even Rules
- Below $50K/yr API → stick with cloud
- $50K–$500K/yr → mixed setup (80% cloud mini + 20% self-hosted 7B)
- Above $500K/yr → GPU cluster + LoRA fine-tune almost always wins

### The Counter-Argument
In 2026, API prices have crashed (DeepSeek $0.14/1M). Can't justify $2K GPU purely on savings. **Real value of self-hosting is privacy, not savings.** If you handle sensitive code or GDPR data, the "cheap" API is actually most expensive due to data leakage.

### Personal Use (OpenClaw + Ollama)
- OpenClaw: FREE | Ollama: FREE | Models: FREE
- Electricity: ~$1-2/day for gaming laptop
- **Total: $0 software + ~$30-60/mo electricity vs $20/mo ChatGPT Plus**

Sources: https://www.ptolemay.com/post/llm-total-cost-of-ownership, https://codersera.com/blog/openclaw-ollama-setup-guide-run-local-ai-agents-2026

---

## The Hybrid Pattern (Emerging Consensus)

Use cheap/free local model (7B-13B) for 70-80% of routine tasks, reserve cloud APIs for 20-30% deep reasoning.

### Evidence
- "Using cheaper model for 70% routine, expensive for 30% yields better ROI than all-in on top model"
- Microsoft "Foundry Local": expose local GPU inference as tool that cloud agent calls, keeping PII local
- OpenClaw's multi-provider architecture is built for this: `agents.defaults.model.primary` switches between providers

### Routing Strategies
1. **By task complexity:** Simple Q&A → local; multi-step reasoning → cloud
2. **By sensitivity:** Code/private data → local; general knowledge → cloud
3. **By confidence:** Start local; escalate to cloud if confidence low
4. **By cost ceiling:** Daily spend caps on cloud, overflow to local

### Hardware Sweet Spot for Hybrid
- Apple Silicon M1 Pro+ with 16-32GB: runs 7B at 2-5 second response
- RTX 4060 Ti (16GB) or RTX 4090 (24GB) for faster inference
- 7B models (Mistral, Llama 3, Qwen) = best speed/quality balance for local tier

Sources: https://medium.com/elevate-tech/a-case-for-localhost-ai-in-2025-local-llm-inference-without-expensive-tokens-0b2838e4ed14, https://techcommunity.microsoft.com/blog/azure-ai-foundry-blog/hybrid-ai-using-foundry-local-microsoft-foundry-and-the-agent-framework---part-1/4470813
