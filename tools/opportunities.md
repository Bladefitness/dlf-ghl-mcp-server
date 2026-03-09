# Opportunities

Pipeline and opportunity management.

## Tools

| Tool | Description |
|------|-------------|
| `ghl_search_opportunities` | Search opportunities with filters |
| `ghl_get_opportunity` | Get an opportunity by ID |
| `ghl_create_opportunity` | Create a new opportunity |
| `ghl_update_opportunity` | Update opportunity details |
| `ghl_delete_opportunity` | Delete an opportunity |
| `ghl_upsert_opportunity` | Create or update an opportunity |
| `ghl_update_opportunity_status` | Change opportunity status (open, won, lost, abandoned) |
| `ghl_add_opportunity_followers` | Add followers to an opportunity |
| `ghl_remove_opportunity_followers` | Remove followers from an opportunity |
| `ghl_list_pipelines` | List all pipelines in a location |
| `ghl_get_pipeline` | Get a pipeline and its stages |

## Disabled Tools

The following tools exist in the codebase but are not active because GHL's API returns 401 for these endpoints regardless of token type:

| Tool | Reason |
|------|--------|
| `ghl_create_pipeline` | GHL API does not permit pipeline creation via API |
| `ghl_update_pipeline` | GHL API does not permit pipeline updates via API |
| `ghl_delete_pipeline` | GHL API does not permit pipeline deletion via API |

Workaround: create and manage pipelines directly in the GHL UI.

## Opportunity Status Values

- `open`
- `won`
- `lost`
- `abandoned`
