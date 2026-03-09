# Sub-Account Management

## Current Accounts

| Name | Location ID | Token Type | Default |
|------|-------------|------------|---------|
| DLF | `W7BRJwzJCvFs9r0xZHrE` | PIV | YES |
| Amazing Skin Care & Med Spa | `RGg9HNyo7e4ttGutRyMt` | PIV | No |
| Christians Testing Account | `2hP6rCb3COd2HUjD25w2` | PIV | No |
| New Heits | `dSgkW9QSUv20b6XIL9H5` | PIV | No |
| TVAAI | `jiR5qR3g4OrMRx6BmpF2` | PIV | No |

## Token Types

**PIV (Private Integration Tokens)** — `pit-xxxxxxxx`
- Static — never expire, no refresh needed
- Generated manually in GHL → Settings → Private Integrations
- Only work for sub-accounts within your own agency

**OAuth Tokens** — JWT
- Generated automatically via the OAuth install flow
- Expire every ~24 hours, auto-refreshed by the server
- Work for any GHL account (including external clients)

## Adding an Account

### Via OAuth Install (automatic)

```bash
curl https://dlf-agency.skool-203.workers.dev/install
# Open the returned URL, select location, authorize
```

### Via MCP Tool

Use `ghl_add_sub_account`:
```
Add sub-account named "Client Name" with location ID "ABC123" and API key "pit-xxxxx"
```

### Via D1 Direct

```bash
npx wrangler d1 execute ghl-accounts --remote --command \
  "INSERT INTO sub_accounts (id, name, api_key, account_type, is_default) VALUES ('LOCATION_ID', 'Name', 'pit-xxxxx', 'sub_account', 0)"
```

## Setting the Default Account

```bash
npx wrangler d1 execute ghl-accounts --remote --command \
  "UPDATE sub_accounts SET is_default = 0; UPDATE sub_accounts SET is_default = 1 WHERE id = 'LOCATION_ID'"
```

## Viewing All Accounts

```bash
npx wrangler d1 execute ghl-accounts --remote --command \
  "SELECT id, name, account_type, is_default, expires_at FROM sub_accounts"
```

Or use the `ghl_list_sub_accounts` MCP tool.

## D1 Schema

```sql
CREATE TABLE sub_accounts (
  id TEXT PRIMARY KEY,         -- GHL Location ID
  name TEXT NOT NULL,
  api_key TEXT NOT NULL,       -- PIV token or OAuth access token
  account_type TEXT DEFAULT 'sub_account',
  is_default INTEGER DEFAULT 0,
  notes TEXT,                  -- Stores "company:COMPANYID" for OAuth accounts
  refresh_token TEXT,          -- OAuth only — null for PIV
  expires_at INTEGER,          -- OAuth only — Unix timestamp, null for PIV
  created_at TEXT,
  updated_at TEXT
)
```
