# Engram MCP

Give your AI agents a memory they can trust. Engram lets your AI remember past conversations, facts, and decisions—so it feels more like a real teammate.

This repository contains configuration templates for connecting MCP clients to [Engram](https://lumetra.io), a hosted memory service for AI agents.

## What is Engram?

Engram is a **hosted MCP server** that provides reliable memory for AI agents:

- **Reliable memory**: Agents remember conversations, facts, and decisions — and can show why results were chosen
- **Easy setup**: Connect via MCP in minutes. Works with Claude Code, Windsurf, Cursor, and other MCP clients
- **Built-in controls**: Manage retention and cleanup with simple tools — no extra plumbing required

**Free during public beta** • No credit card required

## Quick Setup

### 1. Get your API key
Sign up at [lumetra.io](https://lumetra.io) to get your API key.

### 2. Add to your MCP client

**Claude Code:**
```bash
claude mcp add-json engram '{"type":"http","url":"https://engram.lumetra.io","headers":{"X-API-Key":"<your-api-key>"}}'
```

**Windsurf** (`~/.codeium/windsurf/mcp_config.json`):
```json
{
  "mcpServers": {
    "engram": {
      "serverUrl": "https://engram.lumetra.io",
      "headers": {
        "X-API-Key": "<your-api-key>"
      }
    }
  }
}
```

**Cursor** (`~/.cursor/mcp.json` or `.cursor/mcp.json`):
```json
{
  "mcpServers": {
    "engram": {
      "url": "https://engram.lumetra.io",
      "headers": {
        "X-API-Key": "<your-api-key>"
      }
    }
  }
}
```

### 3. Restart your client
Your MCP client will now have access to Engram memory tools.

## Available Tools

Once connected, your agent will have access to these memory tools:

- `store_memory(content)` — Store a single memory
- `store_memories(contents[])` — Store multiple memories at once  
- `search_memories(query, max_candidates?, hints?)` — Search stored memories
- `memory_index(page?, limit?)` — Browse all memories
- `delete_by_event(event_id, dry_run?)` — Delete specific memories
- `explain_retrieval(retrieval_id, verbosity?)` — Understand why results were chosen

## Recommended Agent Prompt

Add this to your agent's system prompt to encourage effective memory usage:

```
You have Engram Memory. Use it aggressively to improve continuity and personalization.

Tools:
- store_memory(content)
- store_memories(contents[])
- search_memories(query, max_candidates?, hints?)
- memory_index(page?, limit?)
- delete_by_event(event_id, dry_run?)
- explain_retrieval(retrieval_id, verbosity?)

Policy:
- Retrieval-first: before answering anything that may rely on prior context, call search_memories (use max_candidates 20–80 for broad queries). Ground answers in results.
- Aggressive storing: capture stable preferences, profile facts, recurring tasks, decisions, and outcomes. Keep each item ≤1–2 sentences. Batch at end of turn with store_memories; use store_memory for single critical facts.
- Cleanup: when info changes, find and delete the old event (memory_index or search_memories → delete_by_event), then store the corrected fact.

Style for stored content: short, declarative, atomic facts.
Examples:
- "User prefers dark mode."
- "User timezone is US/Eastern."  
- "Project Alpha deadline is 2025-10-15."

If asked how results were chosen, call explain_retrieval with the retrieval_id returned by search_memories.
```

## Use Cases

Teams use Engram for:

- **Support with prior context**: Carry forward last ticket, environment, plan, and promised follow‑ups
- **Code reviews with context**: Store ADRs, owner notes, brittle areas, and post‑mortems as memories
- **Shared metric definitions**: Keep definitions, approved joins, and SQL snippets in one place
- **On‑brand content, consistently**: Centralize voice and approved claims for writers

## About This Repository

This repository contains:
- This README with setup instructions for popular MCP clients
- `server.json` - MCP server manifest following the official schema

The `server.json` file uses the official MCP server schema and can be used by MCP clients that support remote server discovery. For manual configuration, use the client-specific examples above.

The actual Engram service runs at `https://engram.lumetra.io` — there's no local installation required.

## Support

- **Product site**: [lumetra.io](https://lumetra.io)
- **Documentation**: See setup instructions above
- **Status**: Free public beta (no credit card required)
