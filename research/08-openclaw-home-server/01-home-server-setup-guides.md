# OpenClaw Home Server Setup Guides — Web Research

## Mac Mini — The Dominant Hardware Pattern

The Mac Mini M4 ($599 base) is overwhelmingly the most-recommended hardware for OpenClaw. Mashable ran an affiliate deal article specifically tying Mac Mini sales to OpenClaw demand. AgentPuter's blog notes Mac Mini units "sold out at Apple Stores in multiple cities."

### Key Specs (consensus across sources)
- Mac Mini M4 base: 16GB unified memory, 256GB SSD — "more than enough"
- Power draw: 5–10W idle, 10–30W under AI workload
- Electricity cost: ~$15–30/year (AgentPuter says $15/yr, AdvenBoost says "under $30/yr", ai.cc says "$1-2/month")

### macOS Always-On Configuration
```bash
sudo pmset -a sleep 0; sudo pmset -a disksleep 0; sudo pmset -a displaysleep 0
sudo pmset -a autorestart 1; sudo pmset -a womp 1
```

### Sources
- Mashable deal article: https://mashable.com/article/feb-4-mac-mini-openclaw-deal
- Beebom step-by-step guide: https://beebom.com/how-to-set-up-clawdbot-moltbot-on-mac-mini/
- AgentPuter deployment guide: https://www.agentputer.com/blog/09-deploy-openclaw/
- AdvenBoost hardware tiers: https://advenboost.com/en/openclaw-setup-the-best-home-server-hardware-for-24-7-ai-agents/
- Medium guide (paywalled): https://thamizhelango.medium.com/transform-your-mac-mini-into-a-personal-ai-powerhouse-the-complete-openclaw-guide-f2f3e8e1b554

---

## Raspberry Pi / ARM Setups

The Pi is well-documented but acknowledged as a "rough edges" path by official docs.

### Hardware Tiers (from Zen van Riel's guide)
- **Pi 3 (1GB)**: Fails at browser automation, unreliable for multi-channel — not recommended
- **Pi 4 (4GB)**: Workable for core functionality, official FAQ confirms it can run the Gateway
- **Pi 5 (8GB)**: The sweet spot — $80 for board, total setup $125-165

### Official FAQ
Gateway needs 512MB-1GB RAM, 1 core, 500MB disk for personal use. Pi 4 can run it.

### Notable Pi Projects
- **Adafruit guide** (learn.adafruit.com): Pi 5 w/ 8GB, TFT display, USB camera, NeoPixels, eSpeak TTS + Whisper STT. Ladyada's note: "Do not give it full leash on billing!" and "not necessarily useful" yet but interesting for hardware/firmware experiments
- **Raspberry Pi official blog** (raspberrypi.com): Endorsed Pi as security-isolating OpenClaw from personal machine — "You gain isolation, control, and peace of mind"
- **Zen van Riel** (zenvanriel.nl): Comprehensive comparison, Pi 5 8GB recommended, electricity under $2/month, NVMe SSD via PCIe recommended for responsiveness

### Key Caveats
- 64-bit OS required, Node.js 22+
- ARM binary compatibility issues exist ("expect rough edges")
- Start with base gateway, add channels incrementally
- Headless is the typical deployment pattern

### Sources
- Zen van Riel: https://zenvanriel.nl/ai-engineer-blog/openclaw-raspberry-pi-setup-hardware-guide/
- Adafruit: https://learn.adafruit.com/openclaw-on-raspberry-pi
- Raspberry Pi official: https://www.raspberrypi.com/news/turn-your-raspberry-pi-into-an-ai-agent-with-openclaw/

---

## Hardware Tiers (AdvenBoost)

1. **Tier 1 — Efficiency King**: Mac Mini M4 Pro (24-48GB unified memory), $1,099-1,899. 10-30W, local inference with Ollama on 7B-13B models.
2. **Tier 2 — Local GPU Beast**: Custom desktop with RTX 4070 Ti Super (16GB VRAM) + Ryzen 9 7950X + 64GB DDR5. Total ~$2,416. Runs 13B-34B models locally.
3. **Tier 3 — Budget Home Lab**: Beelink EQ12 Pro (Intel N100, 16GB RAM, $199-249, 6-15W) or Geekom Mini IT13 (i9-13900H, 32GB, $379-449, 20-45W). Cloud-API only, no local inference.

**Key insight**: OpenClaw itself is lightweight — it's a Gateway that orchestrates cloud API calls, not a local inference engine. You don't need a GPU unless you also want local LLM inference via Ollama.

---

## Power Consumption & Always-On

| Device | Idle/Load Power | Annual Electricity Cost |
|---|---|---|
| Mac Mini M4 | 5-10W idle / 10-30W load | $15-30/year |
| Raspberry Pi 5 | 5-10W typical | Under $24/year |
| Beelink N100 Mini PC | 6-15W | ~$13-26/year |
| Custom GPU desktop (RTX 4070) | 100-285W under load | $200+/year |

### Always-On Essentials (consensus)
- UPS battery backup — CyberPower CP1500PFCLCD recommended ($189-229)
- BIOS "Restore on AC Power Loss" setting
- macOS `autorestart` pmset flag
- Linux: systemd service with `Restart=always`
- `openclaw onboard --install-daemon` creates launchd (macOS) or systemd (Linux) service
- `openclaw doctor` for health checks after reboots

### Migration
Official FAQ confirms you can migrate to a new Mac Mini by copying `~/.openclaw/` directory and running `openclaw doctor`.

---

## Network Setup — Tailscale, SSH, Remote Access

### Official Tailscale Integration (docs.openclaw.ai/gateway/tailscale)

Three modes:
1. **`serve`** — Tailnet-only access (most secure). Gateway stays on loopback, Tailscale handles HTTPS + routing. Supports tokenless auth via Tailscale identity headers.
2. **`funnel`** — Public HTTPS via Tailscale. Requires shared password auth. Supports ports 443, 8443, 10000.
3. **`off`** — Default (no Tailscale automation).

Config example:
```json5
{
  gateway: {
    bind: "loopback",
    tailscale: { mode: "serve" },
  },
}
```

### SSH Tunnel Alternative
```bash
ssh -L 18789:localhost:18789 user@your-server-ip
```

### Security Warning
CVE-2026-25253 demonstrated that exposing the Gateway port directly to the internet enables token exfiltration → RCE. **Never expose port 18789 directly.**

Medium article by Alireza Rezvani ("I Deployed OpenClaw With Zero Public Ports"): Documents scanning OpenClaw instances and finding 8 completely open with full shell access. Advocates Tailscale-only, zero public ports approach.

---

## The "Headless" Setup Pattern

Standard pattern for dedicated hardware:
- Gateway runs as a daemon (launchd on macOS, systemd on Linux)
- No monitor needed — OpenClaw handles browser automation via headless Chrome
- Interact through messaging channels (WhatsApp, Telegram, Slack, etc.) or web dashboard
- Remote management via SSH or Tailscale
- `openclaw onboard --install-daemon` sets this up

### The "Node" Architecture
- Run the Gateway on always-on hardware (Mac Mini, Pi, VPS)
- Pair laptop/phone as "nodes" for screen/camera/canvas access
- Gateway proxies browser actions to nodes
- Recommended hybrid: reliability of always-on + device-specific tools when needed

### Official FAQ
> "Dedicated host (VPS/Mac mini/Pi): always-on, fewer sleep/reboot interruptions, cleaner permissions, easier to keep running."

---

## Real Costs Summary (AgentPuter blog)

| Item | Cost |
|---|---|
| Hardware (Mac Mini M4 base) | $599 one-time |
| API (moderate use) | $15-50/month |
| API (heavy use — Viticci-level) | $100-300/month |
| Electricity | ~$15/year |
| Setup time | 15-30 minutes |

Federico Viticci (MacStories) burned through 180 million tokens with his instance "Navi" — the heavy use case. Most users: $15-50/month.

### Scale
207K GitHub stars, 38K forks, 683 contributors, 12,000+ commits. Project renamed from Clawdbot → MoltBot → OpenClaw in January 2026.
