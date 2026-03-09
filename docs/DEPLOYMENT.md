# Deployment

## Prerequisites

- Node.js 18+
- Cloudflare account with Workers, D1, and KV access
- GHL app credentials (client ID + secret)
- Wrangler CLI: `npm install -g wrangler`

## Initial Setup

```bash
git clone <repo-url>
cd dlf-ghl-mcp-server/dlf-ghl-mcp-server
npm install
npx wrangler login
```

## Local Development

Create `.dev.vars` in `dlf-ghl-mcp-server/` (never commit this file):
```
GHL_API_KEY=pit-xxxxxxxx
GHL_LOCATION_ID=your-location-id
GHL_CLIENT_ID=your-client-id
GHL_CLIENT_SECRET=your-client-secret
ADMIN_PIN=your-pin
```

```bash
npm run dev
# Worker runs at http://localhost:8787
```

## Setting Secrets (Production)

Run each command and type/paste the value when prompted:

```bash
npx wrangler secret put GHL_API_KEY
npx wrangler secret put GHL_LOCATION_ID
npx wrangler secret put GHL_CLIENT_ID
npx wrangler secret put GHL_CLIENT_SECRET
npx wrangler secret put ADMIN_PIN
```

Secrets are encrypted at rest by Cloudflare and never appear in logs or code.

## Deploy

```bash
npm run deploy
```

**Verify:**
```bash
curl https://dlf-agency.skool-203.workers.dev/health
```

Should return:
```json
{ "status": "ok", "server": "GHL MCP Server", "version": "2.0.1", "mcp_endpoint": "/mcp" }
```

## Type Check

```bash
npx tsc --noEmit
```

## Dry Run

```bash
npx wrangler deploy --dry-run --outdir=dist
```

## D1 Database

Schema is auto-migrated at startup. To query directly:
```bash
npx wrangler d1 execute ghl-accounts --remote --command "SELECT id, name, is_default FROM sub_accounts"
```
