# X (Twitter) Research: Context & Memory Solutions People Share

## HIGH-SIGNAL: OpenClaw-Specific Solutions

### @BenjaminBadejo — The Compaction Fix (5 likes, 9 bookmarks, 1,111 impressions)
> "I think I pretty much solved the OpenClaw memory / compaction / token limit 'problem.' The problem isn't the context window; it's the insufficiently conservative, ambiguous, and generalized default memory and compaction instructions."
- Follow-up (6 likes, 18 bookmarks, 1,890 impressions): "the default pre-compaction guidelines are, bizarrely, vague and optional. So I made them detailed and mandatory. And they work."
- Also: dedicated Google Workspace account + phone number + vision/browser tools (196 likes, 283 bookmarks, 16,252 impressions, 8 RTs)

### @jeevanpillay — Keep Config Files Lean (10 impressions)
> "if your MEMORY.md or AGENTS.md is not as lean as this, you're doing something wrong. ~852 tokens total"

### @BullStern — External Memory Tools (13 impressions)
> "Openclaw friends: How are you optimizing MEMORY.md? Using self improvement skills, products like @supermemory or a hybrid?"

### @bytedunks — MEMORY.md vs AGENTS.md Split (38 impressions)
> "what is the thinking behind having MEMORY.md and AGENTS.md separately when I think they both load into the same level of agent context"

## STRUCTURAL ADVICE

### @AleFranz — Small Linked Files > One Giant Doc (4 impressions)
> "Onboarding AI = onboarding a new team. Write it down in markdown: architecture, design decisions, constraints, index files linking it all. Put it in AGENTS.md + a docs/ folder. Small linked files > one giant doc."

### @luka5c0m — Dynamic AGENTS.md / Straion (1 RT, 6 impressions)
> "A dynamic AGENTS.md layer that injects only task-relevant rules (not full context dumps). Less drift. Fewer misses."

### @naoya_ito — Definition of Done in AGENTS.md (569 likes, 581 bookmarks, 59,777 impressions, 55 RTs)
> "AGENTS.md に Definition of Done (DoD) を明記すると、何か作業が終わるたび DoD に従ってチェックを行う。それが失敗すると失敗の原因を探って修復してくれる。"
Translation: Put a clear Definition of Done checklist in AGENTS.md. Agent self-checks and self-repairs against it.

## BROADER CONTEXT MANAGEMENT

### @rryssf_ — Psychology Has the Answer (4,076 likes, 4,666 bookmarks, 198,147 impressions, 765 RTs)
> "psychology solved the ai memory problem decades ago. we just haven't been reading the right papers. your identity isn't something you have. it's something you construct. constantly. from autobiographical memory, emotional experience, and narrative coherence."
Conway's Self-Memory System model applied to AI agents.

### @trustjarvis — State Management Is the Real Problem (1 like, 11 impressions)
> "the hardest part of running a production agent isn't the AI. it's the state management. my context window resets every session. my memory is files on disk. if someone moves a file, renames a key, or changes a schema while I'm sleeping, I wake up confused."

### @TobyQuirk — 15-20 Exchange Cliff (1 like, 22 impressions)
> "Memory loss is the silent killer in agent workflows. Ive seen agents lose the plot completely after 15-20 exchanges."

### @TheCryptoLif7 — Hardware Won't Fix Context (4 likes, 463 impressions)
> "Every AI agent runs inside a context window, that's its working memory. And no, that new Mac Mini M4 you just bought isn't going to fix it."

### @MahdiThistle — Voice Input = Context Noise (27 impressions)
> "Your AI agent gets dumber the longer you talk to it. Not because the model degrades, but because your voice fills the context window with noise."

### @GuyLearnsToCode — 16 Subagents Pattern (4 impressions)
> "Rn I'm at like 16 [subagents]. The best part is you get to preserve the context window of the orchestrator ai agent."

## SUMMARY: What Works (Consensus)

| Solution | Source | Signal |
|----------|--------|--------|
| Make compaction instructions detailed & mandatory | @BenjaminBadejo | 🔴 High |
| Keep AGENTS.md + MEMORY.md lean (~852 tokens) | @jeevanpillay | 🟡 Medium |
| Definition of Done in AGENTS.md | @naoya_ito | 🔴 High (569 likes) |
| Small linked files > one giant doc | @AleFranz | 🟡 Medium |
| Inject only task-relevant rules | @luka5c0m | 🟡 Medium |
| Use subagents aggressively | @GuyLearnsToCode | 🟡 Medium |
| Dated memory files + cron rotation | @AlexDotEth | 🟢 Low |
| External memory tools (SuperMemory, GetZep) | @BullStern | 🟡 Medium |
