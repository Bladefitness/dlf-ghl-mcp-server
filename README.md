# GHL MCP Server

A remote MCP (Model Context Protocol) server that gives AI assistants full programmatic control over GoHighLevel. Built on Cloudflare Workers with ~440 tools organized across every GHL domain.

---

## What This Is

Connect your AI assistant (Claude, Cursor, Windsurf, etc.) to GoHighLevel. Once connected, you can manage your entire GHL account through natural language — no clicking through the UI, no copy-pasting IDs, no manual API work.

**Examples of what you can do:**
- "Find all contacts tagged 'hot lead' added this week and create a follow-up task for each"
- "Create a 5-stage opportunity pipeline called 'Clinic Funnel' and move these 3 contacts into stage 1"
- "Pull the last 20 messages from this conversation and summarize what the prospect said"
- "Update the AI agent prompt for location X and switch it to auto-pilot mode"
- "Generate an invoice for this contact, apply a 10% discount, and send it"

---

## Tool Domains

Tools are organized to match GHL's own structure:

| Domain | Doc | Tools |
|--------|-----|-------|
| Contacts | [contacts/](tools/contacts.md) | Search, create, update, tag, merge, tasks, notes |
| Conversations | [conversations/](tools/conversations.md) | Inbox, messages, send SMS/email/FB/IG, call logs |
| Calendars | [calendars/](tools/calendars.md) | Calendars, groups, appointments, blocked slots, free slots |
| Opportunities | [opportunities/](tools/opportunities.md) | Pipelines, stages, opportunity CRUD, upsert |
| Invoices & Payments | [payments/](tools/payments.md) | Invoices, estimates, schedules, orders, subscriptions, products |
| Marketing | [marketing/](tools/marketing.md) | Campaigns, email builder, email schedules, social planner |
| AI & Automation | [ai/](tools/ai.md) | Conversation agents, voice agents, workflows, knowledge bases |
| Locations | [locations/](tools/locations.md) | Location settings, custom fields, custom values, tags, templates |
| Content | [content/](tools/content.md) | Blogs, funnels, redirects, forms, surveys, courses |
| Businesses | [businesses/](tools/businesses.md) | Business profiles, companies, associations |
| SaaS & Marketplace | [saas/](tools/saas.md) | SaaS plans, rebilling, marketplace app installs |
| Misc | [misc/](tools/misc.md) | Snapshots, links, media, phone numbers |

---

## Connecting to Your AI Assistant

### Claude (claude.ai / Claude Code)

Add to `~/.claude/mcp.json` or your project's `.mcp.json`:

```json
{
  "mcpServers": {
    "ghl": {
      "type": "http",
      "url": "https://your-worker.workers.dev/mcp"
    }
  }
}
```

### Cursor / Windsurf

Add to MCP settings with the same URL format. Most editors support the `http` transport type.

---

## How Sub-Accounts Work

The server manages multiple GHL locations from a single deployment. When you call any tool, you can optionally pass a `locationId` to target a specific account. If you don't, it uses the configured default.

This means one server can serve your entire agency — all locations, all clients, all from one place.

---

## Docs

- [How It Works](docs/architecture.md)
- [Deploying Your Own Instance](docs/deployment.md)
- [Adding Sub-Accounts](docs/accounts.md)
- [Security Model](docs/security.md)
- [Known Limitations](docs/limitations.md)
