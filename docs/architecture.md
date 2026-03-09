# How It Works

## Overview

The server is a Cloudflare Worker that implements the MCP (Model Context Protocol). AI assistants connect to it over HTTP and call tools by name. The server translates those calls into GHL REST API requests and returns the results.

```
AI Assistant
  └── sends tool call: ghl_search_contacts({ query: "John" })
        └── Cloudflare Worker
              └── resolves which GHL location to use
                    └── calls GHL REST API
                          └── returns result to AI
```

## Multi-Location Architecture

The server manages multiple GHL locations from a single deployment. Every tool accepts an optional `locationId` parameter. If provided, that location is used. If not, the configured default is used.

Locations and their tokens are stored in a Cloudflare D1 database. Token refresh is automatic — when an OAuth token is about to expire, the server refreshes it silently before making the API call.

## Tool Structure

Every tool follows the same two-layer pattern:

**Client layer** — async methods that call GHL's REST API directly
**Tools layer** — MCP tool registrations that wrap those methods

This separation makes it easy to add a new tool: add the HTTP method in the client layer, then register it as an MCP tool in the tools layer.

## Request Flow

```
AI tool call
  └── OAuthProvider validates the session
        └── GHLMcpAgent (Durable Object) handles the call
              └── resolveClient(locationId?)
                    └── loads token from D1
                          └── GHLClient calls GHL API
                                └── result returned to AI
```

## Cloudflare Infrastructure

| Component | Purpose |
|-----------|--------|
| Workers | Runs the MCP server — serverless, globally distributed |
| Durable Objects | Maintains MCP session state per connected client |
| D1 (SQLite) | Stores location tokens and sub-account registry |
| KV | Stores OAuth client registrations and admin secrets |
| Wrangler Secrets | Encrypted storage for API keys and credentials |
