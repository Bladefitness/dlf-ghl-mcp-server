# SaaS & Marketplace

SaaS plan management, rebilling, sub-account provisioning, and marketplace app installs.

## SaaS Management

| Tool | Description |
|------|-------------|
| `ghl_saas_get_agency_plans` | Get all agency SaaS plans |
| `ghl_saas_get_plan` | Get a specific SaaS plan |
| `ghl_saas_get_location` | Get SaaS config for a location |
| `ghl_saas_list_locations` | List all SaaS-enabled locations |
| `ghl_saas_enable` | Enable SaaS for a location |
| `ghl_saas_pause` | Pause SaaS for a location |
| `ghl_saas_bulk_enable` | Enable SaaS for multiple locations |
| `ghl_saas_bulk_disable` | Disable SaaS for multiple locations |
| `ghl_saas_get_subscription` | Get SaaS subscription for a location |
| `ghl_saas_update_subscription` | Update a SaaS subscription |
| `ghl_saas_update_rebilling` | Update rebilling settings |

## Sub-Account Management

| Tool | Description |
|------|-------------|
| `ghl_list_sub_accounts` | List all sub-accounts managed by this server |
| `ghl_add_sub_account` | Add a sub-account manually (PIV token) |
| `ghl_remove_sub_account` | Remove a sub-account |
| `ghl_set_default_account` | Set the default sub-account |
| `ghl_update_sub_account_token` | Update the API token for a sub-account |
| `ghl_get_location` | Get a location |
| `ghl_create_location` | Create a new location |
| `ghl_search_locations` | Search locations |

## Marketplace

| Tool | Description |
|------|-------------|
| `ghl_marketplace_list_app_installations` | List app installations |
| `ghl_marketplace_get_app_installation` | Get an app installation |
| `ghl_marketplace_check_has_funds` | Check if an account has funds |
| `ghl_marketplace_create_billing_charge` | Create a billing charge |
| `ghl_marketplace_get_billing_charge` | Get a billing charge |
| `ghl_marketplace_update_billing_charge` | Update a billing charge |
| `ghl_marketplace_delete_billing_charge` | Delete a billing charge |

## Users

| Tool | Description |
|------|-------------|
| `ghl_list_users` | List all users |
| `ghl_get_user` | Get a user |
| `ghl_create_user` | Create a user |
| `ghl_update_user` | Update a user |
| `ghl_delete_user` | Delete a user |
| `ghl_search_users` | Search users |
| `ghl_filter_users_by_email` | Find users by email |

## Snapshots

| Tool | Description |
|------|-------------|
| `ghl_get_snapshots` | List all snapshots |
| `ghl_get_snapshot` | Get a snapshot |
| `ghl_get_last_snapshot_push` | Get the last push for a snapshot |
| `ghl_get_snapshot_push_status` | Check push status |
| `ghl_create_snapshot_share_link` | Create a shareable snapshot link |
