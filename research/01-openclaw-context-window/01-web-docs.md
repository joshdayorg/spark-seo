# Web Research: OpenClaw Context Window Documentation & Guides

## 1. Official Documentation Sources

### docs.openclaw.ai/concepts/context
- "Context" = everything OpenClaw sends to the model for a run, bounded by the model's context window (token limit)
- Context includes: system prompt, conversation history, tool calls/results + attachments
- **System prompt overhead**: ~9,600 tokens (38,412 chars) on every request — fixed cost
- **Tool schemas**: ~8,000 tokens (31,988 chars JSON) — also fixed per request
- **Workspace file injection**: AGENTS.md, SOUL.md, TOOLS.md, IDENTITY.md, USER.md, HEARTBEAT.md, BOOTSTRAP.md
- **Per-file truncation**: `agents.defaults.bootstrapMaxChars` = 20,000 chars default
- **Total injection cap**: `agents.defaults.bootstrapTotalMaxChars` = 150,000 chars default
- Slash commands: `/status` shows context fullness; `/context list` shows injected file sizes; `/context detail` shows per-tool-schema sizes; `/compact` manually compacts

### docs.openclaw.ai/concepts/compaction
- Compaction summarizes older conversation into a compact summary entry, keeps recent messages intact
- Summary persists in session's JSONL history
- Auto-compaction triggers when session nears/exceeds context window
- Before compaction, OpenClaw runs a silent memory flush turn to store notes to disk
- Manual: `/compact` (optionally with focus instructions)
- Config: `agents.defaults.compaction` in openclaw.json

### docs.openclaw.ai/concepts/memory
- Memory = plain Markdown in agent workspace — "the files are the source of truth"
- Two memory layers: `memory/YYYY-MM-DD.md` (daily log) and `MEMORY.md` (curated long-term)
- Tools: `memory_search` (semantic recall via vector index) and `memory_get` (targeted file read)
- Vector search auto-selects provider: local → openai → gemini → voyage → mistral
- QMD backend (experimental): local-first search sidecar combining BM25 + vectors + reranking

### docs.openclaw.ai/concepts/session-pruning
- Pruning trims old tool results from in-memory context before each LLM call — does NOT rewrite on-disk history
- Only active for Anthropic API calls
- `cache-ttl` mode: prunes when last Anthropic call > TTL (default 5m)
- Soft-trim: keeps head + tail of oversized tool results
- Hard-clear: replaces entire tool result with placeholder
- Defaults: `keepLastAssistants: 3`, `softTrimRatio: 0.3`, `hardClearRatio: 0.5`

## 2. Technical Details: Context Sizes & Limits

- **Hard minimum**: 16,000 tokens — models below this throw a `FailoverError`
- **Warning threshold**: 32,000 tokens — logs a warning
- **System prompt + tools + workspace**: consume roughly 20,000–40,000 tokens before first message
- Claude Opus 4.5: 200K tokens
- GPT-5.2 (Codex OAuth): ~266K–272K
- Default fallback: 200,000 tokens

## 3. Blog Posts & Guides

### "8 Ways to Stop Agents from Losing Context" — CodePointer (Substack)
URL: https://codepointer.substack.com/p/openclaw-stop-losing-context-8-techniques
1. Memory Flush Before Compaction
2. Context Window Guards (hard minimum 16K, warning at 32K)
3. Tool Result Guard (synthetic placeholder for orphaned tool calls)
4. Turn-Based History Limiting (cuts at conversation boundaries)
5. Cache-Aware Tool Result Pruning (respects Anthropic prompt caching TTL)
6. Head/Tail Content Preservation
7. Adaptive Chunk Ratio
8. Staged Summarization

### OpenClaw Memory Configuration — Molt Founders
URL: https://moltfounders.com/openclaw-runbook/memory-configuration
- Three pillars: Memory Search, Context Pruning, Compaction
- Cheap embeddings (`text-embedding-3-small`), 6-hour TTL pruning, auto-flush at 40K tokens
- "This one change eliminated most of the 'why did it forget that' moments"

### "OpenClaw's Memory Is Broken. Here's how to fix it" — Daily Dose of DS
URL: https://blog.dailydoseofds.com/p/openclaws-memory-is-broken-heres
- Vector search retrieves similar text but can't reason about relationships
- Solution: Cognee knowledge graph plugin

### Fix context_length_exceeded — LaoZhang AI Blog
URL: https://blog.laozhang.ai/en/posts/openclaw-context-length-exceeded
- System prompt: ~9,600 tokens (fixed)
- Tool schemas: ~8,000 tokens (fixed)
- Workspace injection: up to ~35,600 tokens (GitHub Issue #9157 — "93.5% waste")
- Fixes: `/compact`, `/new`, reduce `bootstrapMaxChars`, disable memory search, increase `reserveTokens`

## 4. Deep Technical Analysis

### Snowan GitBook — Memory System Deep Dive
URL: https://snowan.gitbook.io/study-notes/ai-blogs/openclaw-memory-system-deep-dive
- `MemoryIndexManager` — central class, singleton pattern, per-agent SQLite stores
- Chunking: ~400 tokens/chunk, 80-token overlap, line-aware, SHA-256 hash dedup
- Hybrid search: 70% vector + 30% BM25
- Embedding providers: Local (node-llama-cpp), OpenAI, Gemini
- Batch optimization: OpenAI Batch API = 50% cost reduction

### GitHub Discussion #22251 — Lossless Context Management
URL: https://github.com/openclaw/openclaw/discussions/22251
- Standard compaction = lossy, no temporal structure
- LCM approach: continuous async background summarization forming tree structure
- Summaries of summaries — hierarchical
- Nothing ever thrown away — agent can expand summaries back
- Keeps token use at 30–100K tokens at all times
- Requires custom build (PR #22201 — pluggable context management engines)

## 5. Community Discussions & Pain Points

- Discord: Double compaction on every message with small-context models
- GitHub Issue #2730: "Send notification when context is compacted" — silent compaction is a UX problem
- GitHub Issue #5771: Memory search injection can trigger overflow on fresh sessions
- GitHub Issue #9157: 93.5% waste from workspace file re-injection
- Silent compaction failure (#4747): produces placeholder instead of real summary

## 6. Third-Party Memory Plugins

### Mem0 Plugin (mem0.ai)
- External memory store outside context window
- Auto-Recall: searches before each response
- Auto-Capture: extracts facts after each response
- Install: `openclaw plugins install @mem0/openclaw-mem0`

### Cognee Plugin
- Knowledge graph approach — entities and relationships
- Augments, doesn't replace, markdown memory
