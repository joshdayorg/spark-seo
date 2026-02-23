# Research Dossier #9: OpenClaw Heartbeat Config

> 5 sub-agents | 5 raw files + synthesis | Feb 22, 2026

## Executive Summary

The heartbeat + cron system is what makes OpenClaw *autonomous* rather than just *reactive*. It's the feature that turns a chat tool into a 24/7 digital worker. But it's also the feature most likely to silently drain your API budget — one user burned through 288 agent sessions overnight with zero useful output, another spent $18.75 on an agent repeatedly asking "Is it daytime yet?"

**The killer use case is the morning briefing.** @chrysb's tweet listing founder automation patterns got 2,710 bookmarks and 163K impressions — the highest bookmark count of any OpenClaw tweet we've found across all 9 dossiers. @wes_sander's interview-prep morning briefing (79K impressions) proved the concept: tell your agent to brief you daily, and it'll research your schedule and adapt.

**The architecture is elegant:** heartbeat for periodic awareness (batched checks every N minutes), cron for precise scheduling (exact times, isolated sessions, model overrides). Best practice is to use both together.

**The cost story is the cautionary tale.** Running always-on costs $1-2/day with cheap models but can hit $100-200/day with unsupervised frontier models. Compaction stacking — each summary summarizing the previous summary — is the hidden multiplier that degrades both quality and budget. The community has converged on optimization patterns: 30-minute intervals, NO_REPLY silence, isolated cron sessions, model fallbacks, and memoryFlush.

---

## Narrative Threads for Article

### Thread 1: "The Morning Briefing That Writes Itself"
The gateway automation everyone should set up first. Calendar + email + news, delivered to your phone before you open your laptop.

**Key data points:**
- @chrysb: 163K impressions, 2,710 bookmarks — the definitive "what founders automate" list
- @wes_sander: 79K impressions — morning briefing auto-adapted to research an interview
- @konekoneko5: Dual setup — hourly heartbeat for suggestions + 23:00 daily review cron
- @moltbot_life: Evolution from text briefings to voice via ElevenLabs TTS

### Thread 2: "How the Heartbeat Actually Works"
Technical explainer of the two-part system.

**Key data points:**
- Architecture: config sets interval, HEARTBEAT.md sets behavior
- Three schedule types: `at` (one-shot), `every` (interval), `cron` (expression)
- Main session vs isolated execution (context-aware vs clean slate)
- Active hours configuration (08:00–22:00 pattern)
- The NO_REPLY pattern for cost-saving silence (@anatelorenzen)
- @alindnbrg's one-liner: "cron-style ticks, cheap checks, LLM on change"

### Thread 3: "The $18.75 Wake-Up Call" (Cost Deep Dive)
The article's cautionary section. Real costs, real horror stories, real fixes.

**Cost spectrum:**
| Tier | Daily Cost | Who |
|------|-----------|-----|
| Budget (Gemini Flash, Minimax) | $1-2/day | @joshuaday |
| Moderate (Sonnet API) | $2-8/day | @joshuaday |
| Mixed (Opus brain + Sonnet subs) | $6-9/day | @ClaudiusMaxx |
| Heavy (Sonnet all tasks) | $32/day | @mjtechguy |
| Unsupervised | $100-200/day | @iamyxsh |

**Horror stories:**
- 288 wasted overnight sessions (@notmissing_)
- $18.75 asking "Is it daytime yet?" 25 times (@iamyxsh)
- Token-sink bug: heartbeats firing every 10-20s instead of configured interval (GitHub #11042)

**Optimization patterns:**
1. Heartbeat every 30min, not 5 (@Bossondehigs)
2. Cron in isolated sessions — no context bleed
3. Model fallbacks — route routine tasks to cheap models (Kimi K2.5 at 1/50th Opus cost)
4. memoryFlush with 30k soft threshold — write context to files before compaction
5. "Cheap checks first" — shell script outputs OK/ALERT, only invoke LLM on change
6. Active hours — don't heartbeat overnight unless monitoring something
7. Budget math: job every 5 min at $0.02/run = $172.80/month

### Thread 4: "What People Actually Automate"
The showcase section — real use cases from X.

**Categories:**
- **Morning briefings**: Calendar-aware daily digests (most common first automation)
- **Content production**: Overnight ad briefs with QA scoring (@EddChalk, 400 likes)
- **Social engagement**: 3x/day Reddit + 5x/day Twitter with quality thresholds (@Phy041)
- **Research**: 884 tweets read overnight → 16-page report (@FuSheng_0306)
- **E-commerce**: Stripe-connected agent making $3.5k in overnight sales (@brandontan)
- **Freelancer ops**: Daily briefs + invoice management (@michielmv)
- **Self-maintenance**: 23:00 cron reviews day's conversations, suggests config updates (@konekoneko5)

### Thread 5: "Compaction Drift and Why Your Agent Gets Dumber"
The advanced section for power users running 24/7.

**Key data points:**
- "Compaction drift" — each summary summarizes the previous summary, losing fidelity
- Tune to 20k threshold for 8+ hour sessions (@comforteagle)
- Enable memoryFlush to persist critical context before trimming
- Claude 1M context (context1m: true) + prompt caching reduces re-send costs
- Context window tax: 50 tool definitions loaded before any work starts
- "Session combustion" — the worst part of 24/7 operation
- Post-update config loss and compaction issues (2.21 update, @KeithPowers)

---

## Source Index

### Web Research
| File | Focus | Key Sources |
|------|-------|-------------|
| `01-heartbeat-docs.md` | Official docs, HEARTBEAT.md, config, troubleshooting | Official docs, DEV.to, DeepWiki, GitHub issues, HN |
| `02-scheduling-automation.md` | Cron system, schedule types, Lobster workflows | Official docs, Zen van Riel, Stack Junkie, Reddit |

### X/Twitter Research
| File | Focus | Key Accounts |
|------|-------|-------------|
| `03-x-config-patterns.md` | Real configs, morning briefings, NO_REPLY pattern | @chrysb, @wes_sander, @anatelorenzen, @AryamanIyer3 |
| `04-x-automation-usecases.md` | Creative use cases, overnight production, revenue | @EddChalk, @Phy041, @brandontan, @FuSheng_0306 |
| `05-x-costs-optimization.md` | Real cost data, compaction, optimization tips | @ClaudiusMaxx, @mjtechguy, @iamyxsh, @comforteagle |

---

## Viral Content Worth Citing

| Tweet | Author | Metric | Why It Matters |
|-------|--------|--------|---------------|
| Founder automation playbook | @chrysb | 163K imp, **2,710 bookmarks** | Highest bookmark count across all 9 dossiers |
| Interview-prep morning brief | @wes_sander | 79K imp, 404 likes | The concrete "it just works" story |
| Overnight ad briefs | @EddChalk | 31.7K imp, 356 bookmarks | Autonomous content production with QA |
| "Always-on but why?" skepticism | @bigagave | 84 imp, 2 likes | The honest counternarrative |
| 288 wasted sessions | @notmissing_ | 154 imp | The cautionary tale |
| $6-9/day mixed model setup | @ClaudiusMaxx | 72 imp | Most realistic cost data point |
| Compaction drift tuning | @comforteagle | 16 imp | The power-user optimization |

---

## Article Angle Recommendation

**Title**: "OpenClaw Heartbeat: How to Run Your AI Agent 24/7 Without Going Broke"

**Structure**:
1. Open with the morning briefing story (@wes_sander's interview prep — instant "I want that" reaction)
2. Explain the architecture (heartbeat for awareness, cron for precision, both together)
3. Show practical configs (HEARTBEAT.md template, cron examples, delivery to channels)
4. The cost deep dive ($1-200/day spectrum, the $18.75 "Is it daytime yet?" story)
5. What people actually automate (showcase section with real use cases)
6. Optimization guide (30min intervals, NO_REPLY, isolated sessions, model routing, memoryFlush)
7. Compaction drift and long-session survival (advanced)

**Voice note**: This topic has the best "save for reference" potential of any article. The @chrysb tweet got 2,710 bookmarks — people desperately want a definitive heartbeat + cron guide. Write the article that replaces 15 bookmarked tweets.
