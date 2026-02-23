# Web Research: MCP + OpenClaw Overview

## What MCP Is

**Model Context Protocol (MCP)** is an open standard introduced by Anthropic in November 2024 that enables standardized integration between LLM applications and external data sources/tools.

- **Architecture:** JSON-RPC 2.0 over stateful connections. Three roles: Hosts (LLM apps), Clients (connectors), Servers (services providing context/tools).
- **Inspired by LSP** (Language Server Protocol) — same idea but for AI tools instead of programming language support.
- **Three server primitives:** Resources (data/context), Prompts (templates), Tools (functions for AI to execute).
- **Three client primitives:** Sampling (server-initiated LLM calls), Roots (filesystem boundaries), Elicitation (asking users for info).
- **Often called "USB-C for AI"** (Ars Technica).
- **Governance:** In December 2025, Anthropic donated MCP to the **Agentic AI Foundation (AAIF)** under the Linux Foundation, co-founded by Anthropic, Block, and OpenAI.
- **Adoption:** OpenAI (March 2025), Google DeepMind (April 2025), Replit, Sourcegraph, Zed, Codeium, Cloudflare, Microsoft (Semantic Kernel, Azure OpenAI).
- **SDKs:** TypeScript, Python, Java, Kotlin, C#, Go, PHP, Perl, Ruby, Rust, Swift.

**Sources:** [Anthropic announcement](https://www.anthropic.com/news/model-context-protocol), [MCP spec](https://modelcontextprotocol.io/specification/2025-11-25), [Wikipedia](https://en.wikipedia.org/wiki/Model_Context_Protocol)

---

## MCP + OpenClaw: Current State (Feb 2026)

**MCP support in OpenClaw is NOT yet native/official.** It's the #1 requested feature with multiple open GitHub issues, but user-level MCP server configuration is currently ignored by the gateway.

**Current status from GitHub issue #13248:**

| Feature | Status | Notes |
|---------|--------|-------|
| `mcporter` tool | ✅ Internal use | Used for documentation search |
| `mcpServers` configuration | ❌ Ignored | Logs show "ignoring X MCP servers" |
| `mcpCapabilities.http` | ❌ Disabled | Returns false |
| `mcpCapabilities.sse` | ❌ Disabled | Returns false |
| `agents.defaults.mcp` | ❌ Not supported | Invalid config key |

**Related PRs in progress:**
- `feat(acp): enable MCP tool capabilities #7148` (closed)
- `Fix MCP transport reconnect and SSE header handling #9829` (open)
- `feat(qmd): use MCP server mode instead of subprocess per query #9605` (open)

---

## Community Workarounds (How People Use MCP Today)

### A. freema/openclaw-mcp (MCP Server — exposes OpenClaw TO MCP clients)
- **Direction:** OpenClaw → MCP clients (Claude Desktop, Claude.ai)
- **Architecture:** OpenClaw Gateway ↔ MCP Bridge Server ↔ Claude.ai
- **Auth:** OAuth 2.1, CORS protection
- **Tools exposed:** `openclaw_chat`, `openclaw_status`, `openclaw_chat_async`, `openclaw_task_status`
- **Install:** `npx openclaw-mcp` or Docker
- **Source:** https://github.com/freema/openclaw-mcp

### B. lunarpulse/openclaw-mcp-plugin (MCP Client — connects OpenClaw TO MCP servers)
- **Direction:** OpenClaw → external MCP servers
- **Transport:** Streamable HTTP with SSE
- **Install:** Clone into `~/.openclaw/extensions/`, configure in `openclaw.json` under `plugins.entries`
- **Source:** https://github.com/lunarpulse/openclaw-mcp-plugin

### C. Composio + OpenClaw (3000+ MCP integrations)
- Combined Composio's 3000+ MCP integrations in Docker container
- Adds Slack, GitHub, Google Workspace all accessible via WhatsApp
- Built into product: easyclaw.app

---

## Key Sources

| Source | URL |
|--------|-----|
| Anthropic MCP announcement | https://www.anthropic.com/news/model-context-protocol |
| MCP Specification | https://modelcontextprotocol.io/specification/2025-11-25 |
| GitHub issue #13248 | https://github.com/openclaw/openclaw/issues/13248 |
| GitHub issue #4834 | https://github.com/openclaw/openclaw/issues/4834 |
| freema/openclaw-mcp | https://github.com/freema/openclaw-mcp |
| lunarpulse/openclaw-mcp-plugin | https://github.com/lunarpulse/openclaw-mcp-plugin |
