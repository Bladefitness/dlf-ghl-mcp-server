# Tools Reference

## How Tools Work

Every tool follows the same pattern:

```typescript
server.tool(
  "ghl_tool_name",
  "Description of what it does.",
  {
    locationId: z.string().optional().describe("Target location"),
    name: z.string().describe("Required field"),
  },
  async (args) => {
    try {
      const client = await resolveClient(env, args.locationId);
      const result = await client.domain.methodName(args);
      return ok(`Success\n\n${JSON.stringify(result, null, 2)}`);
    } catch (e: any) {
      return err(e);
    }
  }
);
```

## Tool Naming Rules

- Always `ghl_` prefixed, always snake_case
- Never rename an existing tool — clients cache names and will break
- Never register the same name twice — the worker crashes on startup

## Adding a New Tool to an Existing Domain

1. Add the async method to `src/client/<domain>.ts`
2. Add the `server.tool()` call to `src/tools/<domain>.ts`
3. Grep for the name to confirm no collision:
```bash
grep -r "ghl_your_tool_name" src/tools/
```
4. Type check: `npx tsc --noEmit`
5. Deploy: `npm run deploy`

## Adding a New Domain

1. Create `src/client/<domain>.ts` — export `domainMethods(client: BaseGHLClient)`
2. Create `src/tools/<domain>.ts` — export `registerDomainTools(server, env)`
3. Add to `src/client/index.ts` — import, type, wire in constructor
4. Add to `src/tools/index.ts` — import and call inside `registerAllTools`

## Disabled Tools

Tools that are implemented but not active live in `src/tools/_disabled/`:

| File | Tools | Reason |
|------|-------|--------|
| `pipeline-write.ts` | `ghl_create_pipeline`, `ghl_update_pipeline`, `ghl_delete_pipeline` | GHL returns 401 for all token types |

## Error Auto-Capture

Every tool error is automatically captured. When `result.isError` is true:
- Logged to D1 `ghl_errors` table
- Webhook fired to `ERROR_WEBHOOK_URL` if set

This is fire-and-forget and never blocks the MCP response.
