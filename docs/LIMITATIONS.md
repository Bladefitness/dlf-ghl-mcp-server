# Known Limitations

## Disabled Tools

### Pipeline Write (Create / Update / Delete)

**Tools:** `ghl_create_pipeline`, `ghl_update_pipeline`, `ghl_delete_pipeline`

**Status:** Disabled — implementations preserved in `src/tools/_disabled/pipeline-write.ts`

**What happens:** GHL returns `401 — The token is not authorized for this scope` for `POST/PUT/DELETE /opportunities/pipelines` regardless of token type (PIV, location OAuth JWT, agency PIV).

**What works:** `ghl_list_pipelines` and `ghl_get_pipeline` work fine.

**Workaround:** Create/edit/delete pipelines manually in the GHL UI.

**To re-enable:** Copy tool registrations from `src/tools/_disabled/pipeline-write.ts` back into `src/tools/opportunities.ts` if GHL ever unlocks these endpoints.

---

## GHL App Status

The GHL app (`69adfc954f90be0751e99787`) is currently in **Draft** state. The install URL with `version_id=69adfc954f90be0751e99787` produces `error.noAppVersionIdFound`.

**To fix:** Publish the app version at:
```
https://app.gohighlevel.com/integration/69adfc954f90be0751e99787/versions/69adfc954f90be0751e99787
```

---

## OAuth Token Duration

GHL OAuth tokens expire every ~24 hours. The server auto-refreshes them, but if a refresh_token is ever revoked (user uninstalls app from GHL), that location needs to re-authorize via the install flow.

---

## Single wrangler.toml

`wrangler.toml` only targets the `dlf-agency` worker. The `ghl-mcp-v2` worker uses the same codebase and same D1/KV backends but needs to be deployed separately.
