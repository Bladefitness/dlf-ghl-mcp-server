# DLF GHL MCP Server

A remote MCP (Model Context Protocol) server that gives AI agents full programmatic control over GoHighLevel. Built on Cloudflare Workers with ~440 tools across every GHL API domain.

**Live endpoints:**
- `https://dlf-agency.skool-203.workers.dev/mcp` — primary
- `https://ghl-mcp-v2.skool-203.workers.dev/mcp` — mirror (same codebase)

---

## What This Is

This server sits between AI tools (Claude, Cursor, etc.) and the GoHighLevel API. Once connected, an AI agent can:

- Read and write contacts, conversations, opportunities, invoices, calendars
- Manage automations, workflows, campaigns
- Handle payments, products, subscriptions
- Control AI conversation agents, knowledge bases
- Manage social posts, blogs, funnels
- Add/remove sub-accounts and manage their tokens automatically

All of this happens through natural language — you tell the AI what you want, it calls the right GHL tool.

---

## Documentation

| Doc | What It Covers |
|-----|---------------|
| [Architecture](docs/ARCHITECTURE.md) | How the system is built, request flow, module structure |
| [OAuth & Installs](docs/OAUTH_FLOW.md) | How sub-accounts are added, manual vs automatic |
| [Deployment](docs/DEPLOYMENT.md) | How to deploy, update, and manage the worker |
| [Sub-Account Management](docs/ACCOUNTS.md) | Adding accounts, token types, default account |
| [Security](docs/SECURITY.md) | Admin PIN, token storage, access control |
| [Tools Reference](docs/TOOLS.md) | How tools work, how to add new ones |
| [Marketplace Path](docs/MARKETPLACE.md) | How to open this to external clients |
| [Known Limitations](docs/LIMITATIONS.md) | Disabled endpoints, known GHL API gaps |

---

## Quick Start (Connecting to Claude)

Add to your `~/.claude/mcp.json` or project `.mcp.json`:

```json
{
  "mcpServers": {
    "dlf-agency": {
      "type": "http",
      "url": "https://dlf-agency.skool-203.workers.dev/mcp"
    }
  }
}
```

Restart Claude. You now have access to all ~440 GHL tools.

---

## Managed Sub-Accounts

| Account | Location ID | Default |
|---------|-------------|---------|
| DLF | `W7BRJwzJCvFs9r0xZHrE` | YES |
| Amazing Skin Care & Med Spa | `RGg9HNyo7e4ttGutRyMt` | No |
| Christians Testing Account | `2hP6rCb3COd2HUjD25w2` | No |
| New Heits | `dSgkW9QSUv20b6XIL9H5` | No |
| TVAAI | `jiR5qR3g4OrMRx6BmpF2` | No |

When no `locationId` is specified in a tool call, the default account (DLF) is used automatically.

---

## Stack

- **Runtime:** Cloudflare Workers (Durable Objects)
- **Language:** TypeScript
- **Router:** Hono (via OAuthProvider wrapper)
- **Protocol:** MCP via `@modelcontextprotocol/sdk` + `agents/mcp`
- **Database:** Cloudflare D1 (`ghl-accounts`) — sub-account token registry
- **KV:** Cloudflare KV (`ghl-mcp-oauth`) — OAuth state + admin secrets
- **Auth:** `@cloudflare/workers-oauth-provider`
