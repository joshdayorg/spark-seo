# Running OpenClaw with Local/Self-Hosted LLMs — Web Research

## Official Documentation (docs.openclaw.ai/gateway/local-models)

### Three Configuration Patterns

**a) LM Studio (Recommended by docs)**
- Install LM Studio, download MiniMax M2.1 (largest variant), start server on `http://127.0.0.1:1234`
- Configure provider in `openclaw.json` with `api: "openai-responses"`, set `models.mode: "merge"` to keep cloud fallbacks
- Use Responses API to separate reasoning from final text (important for WhatsApp)

**b) Ollama**
- `ollama launch openclaw` handles everything: installs OpenClaw if missing, configures provider, starts gateway
- `ollama launch openclaw --config` to change model without starting
- Ollama docs recommend **at least 64k token context window** for local models
- Source: https://docs.ollama.com/integrations/openclaw

**c) Any OpenAI-compatible proxy** (vLLM, LiteLLM, KoboldCpp, custom)
- Replace provider block with endpoint/model ID, set `api: "openai-responses"` or `"openai-completions"`, keep `models.mode: "merge"`

### Key Config Patterns
- **Local-first, cloud fallback**: Local model as `primary`, cloud models in `fallbacks` array
- **Cloud-first, local fallback**: Swap order — cloud primary, local fallback
- **Local-only**: Strongest privacy, but lose cloud safety net
- Always keep `models.mode: "merge"` so fallback providers stay registered

### Config Examples
```json
// Hybrid: cloud primary, local fallback
"model": {
  "primary": "anthropic/claude-sonnet-4-5",
  "fallbacks": ["lmstudio/minimax-m2.1-gs32", "anthropic/claude-opus-4-6"]
}

// Local-first
"model": {
  "primary": "lmstudio/minimax-m2.1-gs32",
  "fallbacks": ["anthropic/claude-sonnet-4-5"]
}
```

---

## Performance: Local vs Cloud API

| Metric | Cloud (Claude/GPT) | Local (best case) | Local (typical) |
|--------|--------------------|--------------------|-----------------|
| Response latency | <2s | 0.4–1s (Mac Studio) | 5–30+ seconds |
| Tool calling quality | Excellent | Poor to mediocre | Often broken |
| Context handling | 200k+ tokens | 32–64k usable | Fills up fast |
| Prompt injection defense | Provider-filtered | None (model only) | Weak |

### Real User Reports
- **Tool calling is the #1 problem.** Mac Studio M3 Ultra + 512GB tested GPT-OSS 120B, GLM 4.7 Flash, Qwen 2.5 Coder, Qwen 3 Coder 30B. Result: Qwen 2.5 Coder won't call tools (returns JSON text), others call tools with wrong schemas.
- **Latency compounds.** Ollama → model overhead beyond raw inference. 5+ minute response times reported. llama.cpp gave 16.66x speed boost over Ollama for one user.
- **Context fills fast.** "MiniMax M2.1 works ok, but once context fills up it becomes borderline unusable on Mac."
- **"Sorta functional"** — RTX 5070 Ti 16GB, Qwen3-8B: useful for automation that'd be better served by dedicated tools, not great as general assistant. Had to disable most skills/tools.
- **Security:** Local models skip provider-side safety filters. Docs warn "aggressively quantized or 'small' checkpoints raise prompt-injection risk."

---

## Model Recommendations

### Official Docs
- **MiniMax M2.1** (full-size, via LM Studio) — "Best current local stack"

### NVIDIA Guide (by VRAM)
| VRAM | Model |
|------|-------|
| 8–12GB | qwen3-4B-Thinking-2507 |
| 16GB | gpt-oss-20b |
| 24–48GB | Nemotron-3-Nano-30B-A3B |
| 96–128GB | gpt-oss-120b |

### Ollama Docs
- Cloud: Kimi K2.5, MiniMax M2.5, GLM-5
- Local: GLM-4.7 Flash (~25GB VRAM)

### Community Favorites
- MiniMax M2.1 (best tool calling of local options)
- Qwen3-8B (fits on 16GB, "focused on tool use")
- GPT-OSS 20B (reasoning tasks, poor OpenClaw experience reported)

### What Doesn't Work
- Qwen 2.5 Coder (no tool calling in OpenClaw)
- Any model under ~8B parameters (context too small, reasoning too weak)
- Heavily quantized variants (security risk, poor reliability)

---

## Hardware Requirements

### Official Docs Minimum
> "≥2 maxed-out Mac Studios or equivalent GPU rig (~$30k+). A single 24GB GPU works only for lighter prompts with higher latency."

### Hardware Comparison (from community gist)
| Platform | Response Time | Tokens/sec | 2yr TCO | Best For |
|----------|--------------|------------|---------|----------|
| Raspberry Pi 5 (8GB) | 3–8s | 8 | €150 | Learning only |
| Mac Mini M3 (16GB) | 0.3–0.7s | 45 | €1,100 | Development |
| Cloud VPS (A100/H100) | 0.2–0.5s | 120 | €2,400 | Enterprise |
| Jetson Orin Nano (DIY) | 0.5–1.0s | 22 | €425 | Enthusiasts |
| ClawBox (pre-built Jetson) | 0.7s | 25 | €435 | Always-on assistant |

### Practical Minimums
- **Gateway itself:** Lightweight Node.js, runs on anything
- **Local inference alongside gateway:** 16GB VRAM minimum. 24GB+ preferred.
- **Context window:** 32k minimum (OpenClaw hard min is 16k), 64k recommended
- **"Good" local experience:** Mac Studio 96–192GB unified memory, or multi-GPU with 48GB+ VRAM
- **AMD Developer Cloud:** Free MI300X access ($100 credits ≈ 50 hours) for testing

---

## Sources
1. https://docs.openclaw.ai/gateway/local-models
2. https://docs.ollama.com/integrations/openclaw
3. https://www.nvidia.com/en-us/geforce/news/open-claw-rtx-gpu-dgx-spark-guide/
4. https://www.amd.com/en/developer/resources/technical-articles/2026/openclaw-with-vllm-running-for-free-on-amd-developer-cloud-.html
5. https://www.reddit.com/r/LocalLLM/comments/1qt148w/
6. https://www.reddit.com/r/LocalLLaMA/comments/1qtnz9s/
7. https://www.reddit.com/r/LocalLLaMA/comments/1qwm3wk/
8. https://gist.github.com/yalexx/4f594036b43120a5f3614b2cf83ccc05
