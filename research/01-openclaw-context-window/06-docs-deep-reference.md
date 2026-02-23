# Official Docs Deep Read: OpenClaw Context, Compaction, Memory — Complete Technical Reference

## 1. Context Window Architecture

Every LLM request includes these components:

| Component | Approx Token Cost | Notes |
|---|---|---|
| System prompt | ~9,600 tokens | Fixed per-request |
| Tool schemas | ~8,000 tokens | JSON schemas for all available tools |
| Workspace file injection | Up to ~35,600 tokens | AGENTS.md, SOUL.md, USER.md, etc. — re-sent every message |
| Conversation history | Unbounded (grows) | All accumulated messages |
| Memory search results | Variable | Injected when memorySearch enabled |

**Config:**
- `bootstrapMaxChars`: default 20,000 characters per file
- `bootstrapTotalMaxChars`: default 150,000 characters total
- Context window source: model registry → provider override → default 200,000 tokens

## 2. Compaction Configuration

```json5
{
  agents: {
    defaults: {
      compaction: {
        enabled: true,
        reserveTokens: 16384,
        keepRecentTokens: 20000,
        reserveTokensFloor: 20000,
        memoryFlush: {
          enabled: true,
          softThresholdTokens: 4000,
          systemPrompt: "Session nearing compaction. Store durable memories now.",
          prompt: "Write any lasting notes to memory/YYYY-MM-DD.md; reply with NO_REPLY if nothing to store.",
        },
      },
    },
  },
}
```

- Auto-compaction triggers when: `contextTokens > contextWindow - reserveTokens`
- Community recommendation: increase `reserveTokens` to 40,000 for complex sessions
- Silent compaction failure (#4747): can produce placeholder instead of real summary

## 3. Session Pruning Configuration

```json5
{
  agent: {
    contextPruning: {
      mode: "cache-ttl",
      ttl: "5m",
      tools: { allow: ["exec", "read"], deny: ["*image*"] },
    },
  },
}
```

Defaults:
- `keepLastAssistants`: 3
- `softTrimRatio`: 0.3, `hardClearRatio`: 0.5
- `softTrim.maxChars`: 4000, `headChars`: 1500, `tailChars`: 1500
- Only prunes `toolResult` messages, never user/assistant messages

## 4. Memory System Configuration

```json5
{
  agents: {
    defaults: {
      memorySearch: {
        provider: "openai",
        model: "text-embedding-3-small",
        fallback: "gemini",
        extraPaths: ["../team-docs"],
        remote: {
          batch: { enabled: false, concurrency: 2 },
        },
      },
    },
  },
}
```

- Hybrid search: 70% vector + 30% BM25
- Chunking: ~400 tokens/chunk, 80-token overlap, SHA-256 dedup
- Providers (priority): local → openai → gemini → voyage → mistral

## 5. Session Management

### Session Isolation (`dmScope`)

| Value | Pattern | Use Case |
|---|---|---|
| `main` (default) | `agent:<agentId>:<mainKey>` | Single-user |
| `per-peer` | `agent:<agentId>:dm:<peerId>` | Multi-user isolation |
| `per-channel-peer` | `agent:<agentId>:<channel>:dm:<peerId>` | Multi-user, channel-specific |

### Session Reset
- Daily reset: default 4:00 AM local time
- Idle reset: optional `idleMinutes` sliding window
- Manual: `/new` or `/reset`
- Cron jobs: always fresh `sessionId` per run

### Session Store Fields
- `compactionCount`, `memoryFlushAt`, `memoryFlushCompactionCount`
- `inputTokens`, `outputTokens`, `totalTokens`, `contextTokens`

## 6. Model Context Limits

### Configuration
```json5
{
  agents: {
    defaults: {
      model: {
        primary: "anthropic/claude-sonnet-4-5",
        fallbacks: ["openai/gpt-5.2"],
      },
      contextTokens: 200000,  // cap/minimum
    },
  },
}
```

- Hard minimum: 16,000 tokens (FailoverError below this)
- Warning at: 32,000 tokens
- In-chat switching: `/model` command

## 7. CHANGELOG Highlights (Compaction-Related)

- Restore embedded compaction safeguard/context-pruning extension loading
- Pre-compaction memory flush with configurable soft threshold
- Thread-bound subagents for context isolation
- Session pruning with cache-ttl mode
- Pluggable context management engines (PR #22201)
- Silent compaction failure fix attempts (#4747, #4572, #4121)
