# Managing Sub-Accounts

## How It Works

The server maintains a registry of GHL locations in a Cloudflare D1 database. Each entry has a location ID, display name, and an API token. When a tool call comes in with a `locationId`, the server looks it up and uses the stored token. If no `locationId` is passed, the default account is used.

## Token Types

**PIV tokens** (`pit-xxxxxxxx`) — Private Integration tokens. Static, never expire. Generated in GHL under Settings → Private Integrations. Only work for locations within your own agency.

**OAuth tokens** — Generated automatically via the OAuth install flow. Expire every ~24 hours but are refreshed automatically. Work for any GHL account.

## Adding a Location

### Via OAuth (automatic — works for any GHL account)

```bash
# Get the install URL
curl https://your-worker.workers.dev/install

# Open the URL, authorize in GHL
# The location is added to D1 automatically
```

### Via MCP Tool

Use `ghl_add_sub_account`:
```
Add sub-account with location ID "ABC123", name "Client Name", API key "pit-xxxxx"
```

### Via D1 Direct

```bash
npx wrangler d1 execute ghl-accounts --remote --command \
  "INSERT INTO sub_accounts (id, name, api_key, is_default) VALUES ('LOCATION_ID', 'Name', 'pit-xxxxx', 0)"
```

## Setting the Default

```
Use ghl_set_default_account with locationId "LOCATION_ID"
```

## Viewing All Accounts

```
Use ghl_list_sub_accounts
```
