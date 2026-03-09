# Architecture

## Request Flow

```
AI Agent (Claude, Cursor, etc.)
  └── MCP Client (HTTP)
        └── https://dlf-agency.skool-203.workers.dev/mcp
              └── Cloudflare Worker
                    └── OAuthProvider (default export — must stay as default export)
                          ├── /mcp       → GHLMcpAgent Durable Object
                          │     └── init() → registerAllTools(server, env)
                          │           └── 16 domain modules, ~440 tools
                          │                 └── resolveClient(env, locationId?)
                          │                       └── GHLClient → fetch(GHL REST API)
                          ├── /authorize → OAuth auto-approve
                          ├── /token     → OAuth token endpoint
                          ├── /register  → Dynamic client registration
                          ├── /callback  → GHL OAuth install handler
                          ├── /install   → Returns GHL OAuth install URL
                          ├── /refresh   → Refresh all stored OAuth tokens (PIN protected)
                          ├── /admin/*   → Admin endpoints (PIN protected)
                          └── /*         → Health check, CORS, 404
```

## Two-Layer Module Pattern

Every GHL API domain has two parallel files:

```
src/client/<domain>.ts   — HTTP methods that call GHL REST API
src/tools/<domain>.ts    — MCP tool registrations that call those methods
```

**Client layer** (`src/client/<domain>.ts`):
- Exports a `domainMethods(client: BaseGHLClient)` factory function
- Returns an object of async methods (e.g. `createContact`, `listPipelines`)
- `GHLClient` (`src/client/index.ts`) composes all 16 domain factories in its constructor

**Tools layer** (`src/tools/<domain>.ts`):
- Exports a `registerXxxTools(server: McpServer, env: Env)` function
- Each call to `server.tool()` registers one MCP tool with name, description, Zod schema, and handler
- `registerAllTools` (`src/tools/index.ts`) calls all 16 registration functions during startup

**Shared helpers** (`src/tools/_helpers.ts`):
- `resolveClient(env, locationId?)` — returns a `GHLClient` scoped to the right sub-account
- `ok(text)` — formats a successful MCP response
- `err(e)` — formats an error MCP response

## Sub-Account Resolution

Every tool call goes through `resolveClient`. The resolution order is:

1. `locationId` passed and found in D1 → use its stored token
2. `locationId` passed but not in D1 → use fallback env token with that locationId
3. No `locationId` → use D1 default account (is_default = 1)
4. No default in D1 → fall back to `env.GHL_API_KEY` + `env.GHL_LOCATION_ID`

This means tools work correctly across all sub-accounts without any routing logic in individual tool handlers.

## Domain Modules (16 total)

| Module | Tools file | Client file |
|--------|-----------|-------------|
| accounts | `accounts.ts` | *(uses D1 directly)* |
| ai-agents | `ai-agents.ts` | `ai-agents.ts` |
| automation | `automation.ts` | `automation.ts` |
| businesses | `businesses.ts` | `businesses.ts` |
| calendars | `calendars.ts` | `calendars.ts` |
| contacts | `contacts.ts` | `contacts.ts` |
| content | `content.ts` | `content.ts` |
| conversations | `conversations.ts` | `conversations.ts` |
| knowledge-base | `knowledge-base.ts` | `knowledge-base.ts` |
| locations | `locations.ts` | `locations.ts` |
| marketing | `marketing.ts` | `marketing.ts` |
| marketplace | `marketplace.ts` | `marketplace.ts` |
| misc | `misc.ts` | `misc.ts` |
| opportunities | `opportunities.ts` | `opportunities.ts` |
| payments | `payments.ts` | `payments.ts` |
| saas | `saas.ts` | `saas.ts` |

## Cloudflare Bindings

| Binding | Type | Purpose |
|---------|------|---------|
| `MCP_OBJECT` | Durable Object | `GHLMcpAgent` session state |
| `GHL_DB` | D1 (`ghl-accounts`) | Sub-account token registry |
| `OAUTH_KV` | KV (`ghl-mcp-oauth`) | OAuth state, client registrations, admin secrets |

## Secrets (Wrangler)

| Secret | Purpose |
|--------|---------|
| `GHL_API_KEY` | Fallback PIV token (default sub-account) |
| `GHL_LOCATION_ID` | Fallback location ID |
| `GHL_CLIENT_ID` | GHL OAuth app client ID |
| `GHL_CLIENT_SECRET` | GHL OAuth app client secret |
| `ADMIN_PIN` | PIN to access admin endpoints |

## GHL API Versions

GHL has two API versions. Always match what other methods in the same domain file use:

- `"2021-07-28"` — standard, used by most endpoints
- `"2021-04-15"` — legacy, used by calendar events, blocked slots, Conversation AI, calls, transcriptions

## Critical Rules

- **No duplicate tool names** — `McpServer.tool()` crashes the entire worker on startup if a name is registered twice. Before adding any tool, grep all `src/tools/*.ts` for the name.
- **Tool names are `ghl_` prefixed snake_case** — never rename without updating all MCP clients that reference them.
- **`OAuthProvider` must be the default export** — Cloudflare's binding resolution requires it.
- **All tools registered at `init()` time** — no lazy or conditional registration after startup.
