# Contacts

Full CRUD and management for GHL contacts.

## Tools

| Tool | Description |
|------|-------------|
| `ghl_search_contacts` | Search contacts by name, email, phone, or tags |
| `ghl_get_contact` | Get a contact by ID |
| `ghl_create_contact` | Create a new contact |
| `ghl_update_contact` | Update contact fields |
| `ghl_delete_contact` | Delete a contact |
| `ghl_upsert_contact` | Create or update — matches on email/phone |
| `ghl_list_contacts` | List contacts with filters and pagination |
| `ghl_add_contact_tags` | Add one or more tags to a contact |
| `ghl_remove_contact_tags` | Remove tags from a contact |
| `ghl_bulk_update_contact_tags` | Add/remove tags across multiple contacts at once |
| `ghl_get_contact_notes` | Get all notes on a contact |
| `ghl_create_contact_note` | Add a note to a contact |
| `ghl_update_contact_note` | Edit a note |
| `ghl_delete_contact_note` | Delete a note |
| `ghl_get_contact_tasks` | Get all tasks for a contact |
| `ghl_create_contact_task` | Create a task on a contact |
| `ghl_update_contact_task` | Update a task |
| `ghl_delete_contact_task` | Delete a task |
| `ghl_update_task_completed` | Mark a task complete/incomplete |
| `ghl_get_contact_appointments` | Get all appointments for a contact |
| `ghl_add_contact_to_campaign` | Add contact to a campaign |
| `ghl_remove_contact_from_campaign` | Remove contact from a campaign |
| `ghl_remove_contact_from_all_campaigns` | Remove contact from every campaign |
| `ghl_add_contact_to_workflow` | Enroll contact in a workflow |
| `ghl_remove_contact_from_workflow` | Remove contact from a workflow |
| `ghl_add_contact_followers` | Add followers to a contact |
| `ghl_remove_contact_followers` | Remove followers from a contact |
| `ghl_find_duplicate_contacts` | Find potential duplicate contacts |
| `ghl_merge_duplicate_contacts` | Merge two contacts together |
| `ghl_verify_email` | Verify an email address |
| `ghl_get_contacts_by_business` | Get all contacts linked to a business |

## Usage Notes

- All tools accept an optional `locationId` to target a specific sub-account
- `ghl_upsert_contact` is the recommended tool when you're not sure if a contact exists
- Tags are case-sensitive in GHL
