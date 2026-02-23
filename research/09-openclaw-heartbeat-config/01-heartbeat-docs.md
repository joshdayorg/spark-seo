# OpenClaw Heartbeat System — Official Documentation & Guides

## What Is the Heartbeat?

The heartbeat is OpenClaw's mechanism for **autonomous, periodic agent execution**. It's the feature that differentiates OpenClaw from reactive-only tools like Claude Code. Every N minutes (default: 30), the agent wakes up, reads `~/.openclaw/workspace/HEARTBEAT.md`, runs through its checklist, and either surfaces something important or replies `HEARTBEAT_OK` (which suppresses delivery).

### Key Behaviors
- Runs in the **main session** — full conversational context available
- If `HEARTBEAT.md` is missing or empty, interval heartbeats are **skipped** (cron-tagged reminders still fire)
- Active hours can be configured (e.g., 08:00–22:00) so the agent sleeps overnight
- Active hours with identical start/end are treated as zero-width (always outside window)
- Invalid values like `24:30` are rejected; use `24:00` as end-of-day sentinel

## Configuration

```json5
{
  agents: {
    defaults: {
      heartbeat: {
        every: "30m",
        target: "last",
        activeHours: { start: "08:00", end: "22:00" },
      },
    },
  },
}
```

### HEARTBEAT.md Example
```md
# Heartbeat checklist
- Check email for urgent messages
- Review calendar for events in next 2 hours
- If a background task finished, summarize results
- If idle for 8+ hours, send a brief check-in
```

The agent reads this file every heartbeat cycle and handles all items in one turn.

---

## Heartbeat vs Cron — Decision Matrix

| Use Case | Best Mechanism | Why |
|---|---|---|
| Check inbox every 30 min | Heartbeat | Batches with other checks, context-aware |
| Send daily report at 9am sharp | Cron (isolated) | Exact timing needed |
| Monitor calendar for upcoming events | Heartbeat | Natural fit for periodic awareness |
| Run weekly deep analysis | Cron (isolated) | Standalone task, different model |
| Remind me in 20 minutes | Cron (`--at`) | One-shot with precise timing |
| Background project health check | Heartbeat | Piggybacks on existing cycle |

**Decision flowchart:** Use cron if you need exact timing, session isolation, or a different model. Use heartbeat if the task can batch with other periodic checks and benefits from conversational context. Best practice: use both together.

---

## Cost Considerations (The Big One)

### The Token-Sink Problem (GitHub Discussion #11042)
A community member documented a severe cost trap:
- Heartbeat turns ran with **full main-session context** (170k–210k tokens per heartbeat)
- Heartbeats fired **more frequently than configured** (~10-20s cadence instead of interval) due to system events / exec completions triggering extra runs
- Net effect: massive "do-nothing" turns that still burned huge tokens

### Cost Mitigation Strategies

**Strategy 1: Disable native heartbeat, replace with isolated cron**
```json
{ "agents": { "defaults": { "heartbeat": { "every": "0m" } } } }
```
Then create an isolated cron job with a cheap model for periodic checks.

**Strategy 2: Route heartbeats to a free local model**
Use LM Studio running a small model (3-4B: Qwen 2.5 3B or Phi-3 Mini) for heartbeat duty. Configure with `"cost": { "input": 0, "output": 0 }` so OpenClaw knows it's free.

⚠️ **Known bug** (GitHub #14279): The heartbeat model override can be ignored in some configurations. Workaround: use cron jobs with `--session isolated`.

**Strategy 3: "Cheap checks first" pattern** (DEV.to)
Run a lightweight shell script first that outputs either `HEARTBEAT_OK` or `HEARTBEAT_ALERT`. Only involve a model when there's actual signal. Gives $0 heartbeats most of the time.

### Cost Profile Summary
| Mechanism | Cost Profile |
|---|---|
| Heartbeat | One turn every N minutes; scales with HEARTBEAT.md size + session context |
| Cron (main) | Adds event to next heartbeat (no isolated turn) |
| Cron (isolated) | Full agent turn per job; can use cheaper model |

### Cost Math
A job running every 5 minutes at $0.02/run = **$172.80/month**

---

## Frequency Tuning (DEV.to Recommendations)

| Scenario | Recommended Interval |
|---|---|
| Actively shipping (lots of PRs/CI) | 5–15 minutes |
| Build mode, stable | 30 minutes |
| Just keeping watch | 60–120 minutes |
| Need exact timing | Use cron, not heartbeat |

Start with a one-hour heartbeat. Adjust once you understand consumption.

---

## Common Issues & Troubleshooting

| Problem | Cause | Fix |
|---|---|---|
| Heartbeat not running | `HEARTBEAT.md` missing or empty | Create file with non-empty checklist |
| Heartbeat interrupting subagents | `HEARTBEAT_OK` delivery interrupts work | Structure HEARTBEAT.md so OK only fires when idle |
| Cron shows 'idle' / never runs | Gateway not running or cron disabled | Check `openclaw gateway status`; ensure no `OPENCLAW_SKIP_CRON=1` |
| Messages go to 'system' channel | Missing `deliver: true` | Use explicit delivery settings + isolated session |
| Rapid re-runs beyond interval | System events trigger extra heartbeats | Known issue; use isolated cron as workaround |
| Model override ignored | Bug #14279 | Use cron with `--session isolated` |
| Active hours not working | `24:30` or identical start/end | Use valid times; `24:00` is end-of-day sentinel |

### Debugging Commands
```bash
openclaw gateway status
openclaw cron list
openclaw cron list --verbose
openclaw cron run JOBNAME --force
openclaw logs --follow | grep cron
openclaw doctor
```

---

## Community Perspective (Hacker News)

The debate:
- **Pro:** Heartbeat is what makes OpenClaw autonomous — keeps sessions going for hours
- **Counter:** You could replicate it with `@hourly cd project && claude -p "Get the next task"` — tools in a loop
- **Consensus:** OpenClaw isn't impossible to replicate, but provides easy access to novel features. Real value is pre-built tools and connectors.

---

## Sources
1. Official docs — Cron vs Heartbeat: https://docs.openclaw.ai/automation/cron-vs-heartbeat
2. DEV.to — "Heartbeats in OpenClaw: Cheap Checks First": https://dev.to/damogallagher/heartbeats-in-openclaw-cheap-checks-first-models-only-when-you-need-them-4bfi
3. DeepWiki — Operations and Troubleshooting: https://deepwiki.com/openclaw/openclaw/14-operations-and-troubleshooting
4. GitHub Discussion #11042 — Token sink warning: https://github.com/openclaw/openclaw/discussions/11042
5. GetOpenClaw.ai — Cron & Heartbeat troubleshooting: https://www.getopenclaw.ai/help/cron-heartbeat-automation
6. MrPrompts Substack — Setup guide with heartbeat cost optimization: https://mrprompts.substack.com/p/how-to-set-up-openclaw-your-247-ai
7. Hacker News discussion: https://news.ycombinator.com/item?id=46825228
8. GitHub Issue #14279 — Heartbeat model override bug: https://github.com/openclaw/openclaw/issues/14279
9. GitHub Issue #18573 — Cron announce delivery bug: https://github.com/openclaw/openclaw/issues/18573
