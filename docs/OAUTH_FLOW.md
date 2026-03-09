# OAuth Flow & Sub-Account Installation

## Two Ways to Add a Sub-Account

### 1. Automatic — OAuth Install (recommended for clients)

This is the fully automated path. No manual token handling needed.

**Flow:**
1. Visit `GET /install` — returns the GHL OAuth install URL
2. Share that URL with whoever needs to connect their GHL location
3. They open the URL, select their location in GHL, and click Authorize
4. GHL redirects to your `/callback` endpoint with a one-time auth code
5. `/callback` exchanges the code for a token and stores it in D1 automatically
6. Done — the AI can now access their location immediately

**What gets stored in D1:**
- Location ID
- Access token (scoped to their location)
- Refresh token (for automatic token renewal)
- Expiry timestamp (for auto-refresh logic)

**Token refresh:**
- `resolveClient` checks token expiry before every API call
- If expired or within 5 minutes of expiry, it refreshes automatically
- No manual intervention needed

### 2. Manual — Private Integration Token (PIV)

Used for accounts you own directly. PIV tokens are static — they don't expire and don't need OAuth.

**How to add manually:**

Using the `ghl_add_sub_account` tool or directly via D1:
```sql
INSERT INTO sub_accounts (id, name, api_key, is_default)
VALUES ('LOCATION_ID', 'Account Name', 'pit-xxxxx', 0);
```

**When to use PIV vs OAuth:**
- Your own agency sub-accounts → PIV is fine (simpler)
- External clients → Always use OAuth (fully automatic, no token sharing)

## How the Callback Handles Both App Types

The `/callback` handler is smart — it tries `user_type=Location` first and discriminates based on what GHL returns:

```
GET /callback?code=xxx

Exchange code with user_type=Location:
  ├── Response has locationId → Private Integration flow
  │     Store token directly for that location
  │
  └── Response has companyId → Marketplace/Agency flow
        Store agency token
        Fetch list of installed locations
        Derive location-scoped token for each one
        Store all in D1
```

This means the same callback URL works for both Private Integration apps and Marketplace apps.

## Token Auto-Refresh

For OAuth tokens (not PIV), the server auto-refreshes when needed:

1. `resolveClient` is called for any tool
2. It loads the account from D1
3. `isTokenExpired(account)` checks if `expires_at < now + 5 minutes`
4. If expired → calls `refreshLocationToken(env, locationId)`
5. GHL returns a new access token + refresh token
6. D1 is updated with the new tokens
7. The fresh token is used for the current API call

You can also manually trigger a refresh for all accounts:
```bash
curl -X POST https://dlf-agency.skool-203.workers.dev/refresh \
  -H "X-Admin-Pin: YOUR_PIN"
```
