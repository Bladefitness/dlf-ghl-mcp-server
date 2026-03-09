# Known Limitations

## Pipeline Write

`ghl_create_pipeline`, `ghl_update_pipeline`, and `ghl_delete_pipeline` exist in the codebase but are not registered on the live server. GHL's `POST/PUT/DELETE /opportunities/pipelines` endpoints return 401 for all known token types.

Implementations are preserved in `src/tools/_disabled/pipeline-write.ts` for when GHL unlocks these endpoints.

**Workaround:** Create and manage pipelines directly in the GHL UI.

## OAuth Token Expiry

GHL OAuth tokens expire every ~24 hours. The server auto-refreshes them before they expire. However, if a user uninstalls your app from their GHL account, the refresh token is revoked and that location will need to re-authorize via the install flow.

## Private Integration Scope

PIV tokens only work for locations within your own agency. For external client accounts, use the OAuth install flow.
