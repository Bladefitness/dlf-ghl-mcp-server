# Security

## Admin PIN

All admin endpoints require an `X-Admin-Pin` header. The PIN is stored as a Wrangler secret — encrypted at rest, never in code or logs.

**Protected endpoints:**
- `GET /admin/agency-token` — view masked agency token
- `POST /admin/agency-token` — rotate agency token
- `POST /refresh` — refresh all OAuth tokens

```bash
curl -H "X-Admin-Pin: YOUR_PIN" https://your-worker.workers.dev/admin/agency-token
```

## Token Storage

All tokens are stored in Cloudflare D1 and KV, both encrypted at rest. Tokens are never written to logs — the logger has a redact list that strips known secret field names.

## Rotating Credentials

**Rotate admin PIN:**
```bash
npx wrangler secret put ADMIN_PIN
```

**Rotate agency token via API:**
```bash
curl -X POST https://your-worker.workers.dev/admin/agency-token \
  -H "X-Admin-Pin: YOUR_PIN" \
  -H "Content-Type: application/json" \
  -d '{"token":"pit-new-token"}'
```

## Public Endpoints

These are intentionally public:
- `GET /health` — no sensitive data
- `GET /install` — returns OAuth install URL only
- `GET /callback` — GHL redirects here after OAuth
- `GET /userinfo` — required for GHL External Auth
