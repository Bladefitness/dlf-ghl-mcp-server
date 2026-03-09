# Security

## Admin PIN

All administrative endpoints require the `X-Admin-Pin` header. The PIN is stored as a Wrangler secret (`ADMIN_PIN`) — encrypted at rest, never in code or logs.

**Protected endpoints:**

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/admin/agency-token` | GET | View masked agency PIV token |
| `/admin/agency-token` | POST | Rotate/store a new agency PIV token |
| `/refresh` | POST | Refresh all stored OAuth tokens |

**Usage:**
```bash
curl -H "X-Admin-Pin: YOUR_PIN" https://dlf-agency.skool-203.workers.dev/admin/agency-token
```

## Token Storage

| Token | Where Stored | Encrypted |
|-------|-------------|----------|
| Sub-account PIV/OAuth tokens | D1 `sub_accounts.api_key` | Cloudflare D1 at rest |
| OAuth refresh tokens | D1 `sub_accounts.refresh_token` | Cloudflare D1 at rest |
| Agency PIV token | KV `agency:pit_token` | Cloudflare KV at rest |
| OAuthProvider client registrations | KV `client:*` | Cloudflare KV at rest |
| Secrets (PIN, client secret, etc.) | Wrangler Secrets | Cloudflare Secrets |

Tokens are never logged. The logger has a redact list that strips known secret field names from all output.

## Rotating the Admin PIN

```bash
npx wrangler secret put ADMIN_PIN
```

## Rotating the Agency PIV Token

```bash
curl -X POST https://dlf-agency.skool-203.workers.dev/admin/agency-token \
  -H "X-Admin-Pin: YOUR_PIN" \
  -H "Content-Type: application/json" \
  -d '{"token":"pit-new-token-here"}'
```

## Viewing the Current Agency Token (Masked)

```bash
curl -H "X-Admin-Pin: YOUR_PIN" https://dlf-agency.skool-203.workers.dev/admin/agency-token
# Returns: {"token":"pit-5f170ee5***9c36"}
```

## Public Endpoints (Intentional)

- `GET /health` — no sensitive data
- `GET /install` — returns OAuth install URL only
- `GET /callback` — must be public for GHL to redirect to
- `GET /userinfo` — must be public for GHL External Auth
