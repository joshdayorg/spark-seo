# Research Dossier: OpenClaw Context Window

**Topic:** "OpenClaw Context Window: How to Stop Your Agent from Getting Dumber"
**Target keyword:** `openclaw context window`
**Research date:** February 22, 2026
**Sub-agents used:** 6

---

## Executive Summary

The context window is the single biggest pain point for active OpenClaw users. When a session fills to ~150K tokens, the agent visibly degrades — forgetting instructions, hallucinating completed tasks, and ignoring skills. Most users don't realize this is happening until the damage is done.

The problem is well-understood but poorly documented. Official docs cover the mechanics; community wisdom covers the workarounds. Nobody has stitched both into a definitive guide.

---

## The Core Problem

**Every OpenClaw message costs ~18,000-40,000 tokens before you even type anything:**
- System prompt: ~9,600 tokens (fixed)
- Tool schemas: ~8,000 tokens (fixed)
- Workspace files (AGENTS.md, SOUL.md, etc.): up to ~35,600 tokens
- That's your "tax" on every single message

**What happens when context fills up:**
- At ~130-150K tokens: quality noticeably degrades ("Lost in the Middle" effect)
- At context limit: auto-compaction fires — summarizes old conversation, keeps recent messages
- After compaction: agent loses details that weren't persisted to disk ("compaction amnesia")
- Symptoms: forgets instructions, claims to have done things it didn't, ignores skills, substitutes its own approach

**The compounding problem:**
- Each compaction cycle loses fidelity
- Sessions >8 hours need more aggressive thresholds (~20K vs default 30K)
- @comforteagle: "Fidelity loss compounds fast at higher compaction frequency"

---

## What the Benchmarks Say

| Finding | Source |
|---------|--------|
| 11/12 models drop below 50% performance at 32K tokens | NoLiMa Benchmark (arXiv) |
| Models recall beginning and end well, but lose the middle | Stanford "Lost in the Middle" |
| Claude Sonnet 4: 97.2% at 8K → 81.3% at 120K | Fiction.liveBench |
| GPT-5: 100% at 8K → 87.5% at 192K | Fiction.liveBench |
| Llama-3.1-70B: 719% latency increase at 15K words | Chandran & Hossain (arXiv) |
| Practical limit is ~80% of advertised context | AIMultiple, Demiliani testing |

---

## The Winning Solutions (Ranked by Community Signal)

### Tier 1: High-signal, proven

1. **Orchestrator + Subagents** (577 likes, 1,556 bookmarks — @johann_sath)
   - Add "you are the orchestrator. subagents execute." to AGENTS.md
   - Main session stays lean, heavy work runs in fresh context windows
   - Thread-bound subagents shipped in OpenClaw 2026.2.21

2. **Definition of Done in AGENTS.md** (569 likes, 581 bookmarks — @naoya_ito)
   - Agent self-checks and self-repairs against explicit checklist
   - "The first step of harness engineering"

3. **Make compaction instructions detailed & mandatory** (18 bookmarks, article — @BenjaminBadejo)
   - Default pre-compaction guidelines are "vague and optional"
   - Making them specific and mandatory "eliminated forgetfulness"

4. **Memory "tattoos"** — persistent .md files that survive restarts (@rocklobster_45, @R44D)
   - Rolling memory file updated after every session
   - "Context rebuilds automatically on restart"

### Tier 2: Medium-signal, validated

5. **Lean workspace files** (~852 tokens combined for AGENTS.md + MEMORY.md — @jeevanpillay)
6. **Small linked files > one giant doc** (index + docs/ folder — @AleFranz)
7. **Dynamic context injection** — only task-relevant rules, not full dumps (@luka5c0m / Straion)
8. **Local Ollama for heartbeats** — free, stops token bleed (@ziwenxu_)
9. **Discord channels as session isolation** — each channel = own session (@Flippin4Gold)

### Tier 3: Emerging approaches

10. **Lossless Context Management** — hierarchical tree summarization, nothing thrown away (GitHub #22251)
11. **Mem0 plugin** — external memory store that survives compaction
12. **Cognee knowledge graph** — entities + relationships instead of flat text
13. **Conway's Self-Memory System** — psychological model for agent identity (198K impressions — @rryssf_)

---

## Key Config Settings

| Setting | Default | Recommendation |
|---------|---------|----------------|
| `compaction.reserveTokens` | 16,384 | Increase to 40,000 for complex work |
| `compaction.keepRecentTokens` | 20,000 | Keep default or lower for long sessions |
| `bootstrapMaxChars` | 20,000 | Reduce to 10,000-5,000 to save context |
| `contextPruning.mode` | `cache-ttl` | Enable with 5-6 hour TTL |
| `contextPruning.keepLastAssistants` | 3 | Keep default |
| `session.reset.mode` | `daily` | 4 AM reset prevents unbounded growth |
| `memorySearch.provider` | auto | Use `text-embedding-3-small` for cheapest |

---

## Article Angle

**"Your agent isn't broken, it's full."**

The article nobody has written: a practical, experience-based context management playbook. Not a config dump. Not a docs mirror. The article that explains:
- Why your agent gets dumber after an hour (with the actual numbers)
- The 18,000-token tax you're paying on every message
- The orchestrator + subagents pattern that 1,556 people bookmarked
- Exact config changes with before/after
- When to start a new session vs. when to compact
- How to structure memory so restarts don't hurt

**Competition gap:** Existing articles are either dry config references (LaoZhang, Molt Founders) or surface-level tips (CodePointer). Nobody has synthesized the academic research, the official docs, AND the community wisdom into a single actionable guide.

---

## Raw Research Files

| File | Contents |
|------|----------|
| `01-web-docs.md` | Official docs, blog posts, guides, community discussions |
| `02-web-technical.md` | Academic benchmarks, memory systems, best practices |
| `03-x-pain-points.md` | User complaints and symptoms from X |
| `04-x-solutions.md` | Tips, workarounds, and solutions shared on X |
| `05-x-stories.md` | Real user experiences with sessions and context |
| `06-docs-deep-reference.md` | Complete config reference from official docs |
