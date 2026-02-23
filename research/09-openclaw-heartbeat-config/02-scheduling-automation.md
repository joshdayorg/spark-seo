# OpenClaw Scheduling, Cron & Automation System — Web Research

## The Built-In Cron System

OpenClaw has a **built-in cron scheduler** that runs inside the Gateway process — not inside the LLM. Jobs persist at `~/.openclaw/cron/jobs.json` and survive restarts. Management via CLI (`openclaw cron add/edit/list/run/remove`) or by chatting with the agent (tool-call access to `cron.add`, `cron.update`, `cron.run`, `cron.remove`).

### Three Schedule Types

1. **`at`** — One-shot at specific ISO 8601 timestamp. Auto-deletes after success. Use for reminders ("remind me in 20 minutes").
2. **`every`** — Fixed interval in milliseconds (e.g., 3600000 = 1 hour). For monitoring/polling where exact clock alignment doesn't matter.
3. **`cron`** — Standard 5-field (or 6-field with seconds) cron expressions with optional IANA timezone. Full precision: `0 7 * * *` = every day at 7 AM, `0 9 * * 1` = Mondays at 9 AM.

### Load Spreading
Recurring top-of-hour expressions (e.g., `0 * * * *`) get a deterministic per-job stagger of up to 5 minutes to avoid load spikes. Fixed-hour expressions like `0 7 * * *` remain exact. Override with `--stagger <duration>` or `--exact`.

---

## Execution Modes

### Main Session Jobs
Enqueue a system event into the heartbeat runner. Agent runs with full conversation context. Two wake modes:
- `wakeMode: "now"` — triggers immediate heartbeat
- `wakeMode: "next-heartbeat"` — waits for next scheduled heartbeat

### Isolated Jobs
Spin up a fresh session (`cron:<jobId>`) with no conversation history.
- No pollution of main chat
- Can override model and thinking level per job
- Each run starts clean
- Default delivery mode is `announce` (posts summary to a channel)

### Comparison Table
| | Heartbeat | Cron (main) | Cron (isolated) |
|---|---|---|---|
| Session | Main | Main (via system event) | `cron:<jobId>` |
| History | Shared | Shared | Fresh each run |
| Context | Full | Full | None (clean) |
| Model | Main session | Main session | Can override |

**Important:** Main session cron jobs require heartbeats to be enabled. If heartbeats are disabled, use `--session isolated` for all cron jobs.

---

## Practical Examples

### Morning Briefing (7 AM daily)
```bash
openclaw cron add \
  --name "Morning briefing" \
  --cron "0 7 * * *" \
  --tz "America/New_York" \
  --session isolated \
  --message "Generate today's briefing: weather, calendar, top emails, news summary." \
  --model opus \
  --announce \
  --channel whatsapp \
  --to "+15551234567"
```

### One-Shot Reminder
```bash
openclaw cron add \
  --name "Meeting reminder" \
  --at "20m" \
  --session main \
  --system-event "Reminder: standup meeting starts in 10 minutes." \
  --wake now \
  --delete-after-run
```

### Weekly Cost Report (Mondays 8 AM)
Queries usage logs, calculates spend by model, compares week-over-week, highlights anomalies.

### Hourly Git Backup
Runs `git add -A`, commits with timestamp, pushes. Skips if nothing changed.

### Nightly Research Sweep (2 AM)
Searches for new content on tracked topics, saves to knowledge base, sends digest.

---

## Delivery & Channel Integration

Isolated cron jobs can deliver output directly to channels:
- **Announce mode**: Posts output to Slack, Discord, Telegram, WhatsApp, Signal, iMessage, Mattermost
- **Webhook mode**: POSTs JSON payload to HTTP(S) endpoint
- **None**: Internal only, no delivery

Telegram supports forum topics via the `:topic:` syntax in delivery targets.

---

## Lobster: Deterministic Workflows with Approvals

For tasks more complex than a single agent turn, OpenClaw has **Lobster** — a workflow runtime for multi-step tool pipelines with human approval gates and resumable runs. Cron or heartbeat triggers *when* a run happens; Lobster defines *what steps* happen.

---

## Configuration

```json5
{
  cron: {
    enabled: true,
    store: "~/.openclaw/cron/jobs.json",
    maxConcurrentRuns: 1,
  },
}
```

Disable entirely with `cron.enabled: false` or `OPENCLAW_SKIP_CRON=1`.

---

## Integration with External Scheduling

- **systemd timers**: Community members use systemd alongside OpenClaw for background services (e.g., email drip engines every 6 hours). Reddit recommends "systemd > nohup" for persistent processes.
- **launchd (macOS)**: Gateway runs as daemon via launchd. Internal scheduling handles agent tasks, so external launchd/cron only needed for starting/stopping the Gateway itself.
- **No need for external cron**: OpenClaw's built-in cron replaces traditional crontab for agent tasks. Persists jobs and survives restarts.

---

## Common Gotchas (Community)

- **Timezone confusion**: Cron defaults to host timezone; always specify `--tz`
- **Token costs**: A job running every 5 minutes at $0.02/run = $172.80/month
- **Testing**: Always test payloads in isolated sessions before scheduling
- **The `deleteAfterRun` flag**: Reddit users reported cron jobs not triggering until explicitly set and gateway restarted

---

## Sources
1. Official docs — Cron Jobs: https://docs.openclaw.ai/automation/cron-jobs
2. Official docs — Cron vs Heartbeat: https://docs.openclaw.ai/automation/cron-vs-heartbeat
3. Zen van Riel — Building Proactive AI Automation: https://zenvanriel.nl/ai-engineer-blog/openclaw-cron-jobs-proactive-ai-guide/
4. Stack Junkie — 3 Proven Methods: https://www.stack-junkie.com/blog/openclaw-cron-jobs-automation-guide
5. OpenClaw Task Automation Tutorial: https://openclaw-ai.online/tutorials/use-cases/task-automation/
6. Reddit r/AI_Agents — Cron job setup: https://www.reddit.com/r/AI_Agents/comments/1quqd5m/
7. GitHub Releases (stagger feature): https://github.com/openclaw/openclaw/releases
