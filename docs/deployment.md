# Deploying Your Own Instance

## Prerequisites

- Cloudflare account (free tier works)
- Node.js 18+
- A GHL account with a Private Integration or Marketplace app set up
- Wrangler CLI: `npm install -g wrangler`

## Setup

```bash
git clone https://github.com/Bladefitness/dlf-ghl-mcp-server
cd dlf-ghl-mcp-server/dlf-ghl-mcp-server
npm install
npx wrangler login
```

## Configure Secrets

```bash
npx wrangler secret put GHL_API_KEY        # Your GHL PIV token
npx wrangler secret put GHL_LOCATION_ID   # Your default GHL location ID
npx wrangler secret put GHL_CLIENT_ID     # GHL OAuth app client ID
npx wrangler secret put GHL_CLIENT_SECRET # GHL OAuth app client secret
npx wrangler secret put ADMIN_PIN         # Your chosen admin PIN
```

## Deploy

```bash
npm run deploy
```

## Verify

```bash
curl https://your-worker.workers.dev/health
# Should return: { "status": "ok", "mcp_endpoint": "/mcp" }
```

## Local Development

Create `.dev.vars` (never commit this):
```
GHL_API_KEY=pit-xxxxxxxx
GHL_LOCATION_ID=your-location-id
GHL_CLIENT_ID=your-client-id
GHL_CLIENT_SECRET=your-client-secret
ADMIN_PIN=your-pin
```

```bash
npm run dev
```

## Type Check

```bash
npx tsc --noEmit
```
