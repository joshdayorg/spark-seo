# OpenClaw VPS & Cloud Hosting Options — Web Research

## Sources Reviewed (10+ articles, 5 read in full)
1. Official Docs — VPS Hub (docs.openclaw.ai/vps)
2. Official Docs — Docker Guide (docs.openclaw.ai/install/docker)
3. Official Docs — Hetzner Guide (docs.openclaw.ai/install/hetzner)
4. Official Docs — Oracle Cloud Guide (docs.openclaw.ai/platforms/oracle)
5. Official Docs — Railway Guide (docs.openclaw.ai/install/railway)
6. Official Docs — Northflank Guide (docs.openclaw.ai/install/northflank)
7. Official Docs — Fly.io Guide (docs.openclaw.ai/install/fly)
8. DigitalOcean Tutorial (digitalocean.com/community/tutorials/how-to-run-openclaw)
9. Apiyi Cloud vs Local Comparison (help.apiyi.com)
10. Cognio Self-Hosting Guide (cognio.so) — Oracle free tier + Ollama
11. Microsoft Security Blog (microsoft.com/security/blog)
12. Reddit r/LocalLLaMA — Cloudflare Workers approach vs VPS
13. Reddit r/selfhosted — 6 CVEs, 42,000+ exposed instances, security incidents

---

## VPS & Cloud Provider Options

### Tier 1: One-Click / No Terminal (Easiest)

| Provider | Cost | What You Get | Setup Time |
|----------|------|-------------|------------|
| **Railway** | Usage-based, ~$5-20/mo | One-click deploy, `/setup` web wizard, persistent volume at `/data`, backup export | 5 min |
| **Northflank** | Free tier available | One-click deploy, web wizard at `/setup`, persistent volume | 5 min |
| **DigitalOcean 1-Click** | $24/mo (4GB RAM) | Pre-built marketplace image, security hardened, Docker isolation, DM pairing | 5 min |
| **DigitalOcean App Platform** | Variable | Auto-scaling, zero-downtime deploys, sleep mode for cost optimization | 10 min |

### Tier 2: Docker VPS (More Control)

| Provider | Cost | Specs | Notes |
|----------|------|-------|-------|
| **Hetzner** | ~$4-5/mo (CX22) | 2 vCPU, 4GB RAM | Official guide in docs. Cheapest reliable paid option. Docker + SSH tunnel. |
| **Oracle Cloud Free Tier** | $0 forever | 4 ARM OCPU, 24GB RAM, 200GB SSD | Official guide in docs. Best free option. ARM arch (some binary limitations). Uses Tailscale for secure access. |
| **Vultr** | $6/mo | 1 vCPU, 1GB RAM | Many regions. Community guides available. |
| **Linode/Akamai** | $5/mo | 1 vCPU, 1GB RAM | Standard Linux VPS approach. |
| **AWS EC2/Lightsail** | Variable | Free tier available | Video guide linked from docs. |

### Tier 3: Container Platforms

| Provider | Cost | Notes |
|----------|------|-------|
| **Fly.io** | Free tier + ~$5-10/mo | Official guide. Persistent volume, auto-HTTPS, `flyctl` CLI. 2GB RAM recommended. |
| **Cloudflare Workers** | $5/mo subscription | Community approach (moltworker). Isolated, secure, admin panel included. |
| **GCP Compute Engine** | Variable | Official guide in docs. |

---

## Docker Deployment Details

### Two Official Modes

1. **Containerized Gateway** — Full OpenClaw in Docker. Run `./docker-setup.sh` from repo root. Builds image, runs onboarding wizard, starts via Docker Compose. Config persists at `~/.openclaw/` on host.

2. **Agent Sandbox** — Host gateway stays on bare metal, but agent tools run inside Docker containers for isolation. Default: one container per agent, `network: none`, read-only root, cap-drop ALL, non-root user, 1GB memory limit.

### Key Docker Settings
- `OPENCLAW_DOCKER_APT_PACKAGES` — bake system packages into image
- `OPENCLAW_EXTRA_MOUNTS` — add host bind mounts
- `OPENCLAW_HOME_VOLUME` — persist `/home/node` in named volume
- Image runs as uid 1000 (node user)
- Hetzner guide emphasizes: **never install binaries at runtime** — they vanish on restart. Bake everything into the Dockerfile.

---

## Home Server vs VPS Comparison

| Dimension | Home Server | VPS/Cloud |
|-----------|-------------|-----------|
| **Upfront cost** | $80 (Pi) to $599+ (Mac Mini) | $0 (Oracle free tier) to $24/mo |
| **Ongoing cost** | Electricity only (~$15-30/yr) | $4-24/month |
| **Setup complexity** | Moderate (15-30 min) | Easy (5 min for one-click) to moderate |
| **Performance** | Excellent (dedicated hardware) | Shared, varies by tier |
| **Data control** | Full local control | Provider has physical access |
| **Uptime** | Depends on power/internet | Provider SLA (99.9%+) |
| **Maintenance** | Self-managed updates, hardware | Managed infrastructure |
| **Browser automation** | Native (macOS) | Headless Chrome only |
| **Local LLMs** | Possible (Mac Mini + Ollama) | Generally not feasible |

---

## Security Considerations

### CVE History (from Microsoft Security Blog & Reddit)
- 6 CVEs documented to date
- 42,000+ exposed OpenClaw instances found in internet scans
- CVE-2026-25253: Token exfiltration → RCE via exposed Gateway port
- Supply chain poisoning via malicious skills

### Best Practices
1. Never expose port 18789 directly to the internet
2. Use Tailscale `serve` mode or SSH tunnels
3. Enable Docker sandboxing for agent tool execution
4. Regular updates (`openclaw update`)
5. Review skills before installing (check source code)

### Official Security Model
- Gateway binds to loopback by default
- Shared password authentication when accessed remotely
- Tailscale identity headers for tokenless auth on tailnet
- Agent sandboxing: network isolation, read-only root, capability drops

---

## Key Insight

The VPS path is **cheaper and easier** for users who just want an always-on agent with messaging channels. The home server path (Mac Mini) makes sense when you want:
- Local LLM inference via Ollama
- Native macOS tools (browser, camera, screen)
- Full data sovereignty
- Zero ongoing hosting costs beyond electricity

Oracle Cloud free tier is the best-kept secret — 24GB RAM, completely free, official docs support it.
