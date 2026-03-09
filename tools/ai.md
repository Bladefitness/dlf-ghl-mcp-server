# AI & Automation

Conversation AI agents, voice agents, workflows, and knowledge bases.

## Conversation AI Agents

| Tool | Description |
|------|-------------|
| `ghl_search_conversation_agents` | Search conversation AI agents |
| `ghl_get_conversation_agent` | Get an agent by ID |
| `ghl_create_conversation_agent` | Create a new conversation agent |
| `ghl_update_conversation_agent` | Update an agent (prompt, mode, channels, etc.) |
| `ghl_delete_conversation_agent` | Delete an agent |
| `ghl_get_conversation_generation` | Get the current generation/response from an agent |
| `ghl_update_followup_settings` | Update agent follow-up settings |

## Conversation Actions (Agent Tools)

| Tool | Description |
|------|-------------|
| `ghl_list_conversation_actions` | List all actions on an agent |
| `ghl_get_conversation_action` | Get a specific action |
| `ghl_create_conversation_action` | Add an action to an agent |
| `ghl_update_conversation_action` | Update an action |
| `ghl_delete_conversation_action` | Remove an action |

## Voice AI Agents

| Tool | Description |
|------|-------------|
| `ghl_list_voice_agents` | List all voice agents |
| `ghl_get_voice_agent` | Get a voice agent |
| `ghl_create_voice_agent` | Create a voice agent |
| `ghl_update_voice_agent` | Update a voice agent |
| `ghl_delete_voice_agent` | Delete a voice agent |
| `ghl_get_voice_action` | Get a voice agent action |
| `ghl_create_voice_action` | Create a voice agent action |
| `ghl_update_voice_action` | Update a voice agent action |
| `ghl_delete_voice_action` | Delete a voice agent action |

## Agent Studio

| Tool | Description |
|------|-------------|
| `ghl_list_agent_studio_agents` | List Agent Studio agents |
| `ghl_get_agent_studio_agent` | Get an Agent Studio agent |
| `ghl_execute_agent_studio_agent` | Execute an Agent Studio agent |

## Knowledge Bases

| Tool | Description |
|------|-------------|
| `ghl_kb_list` | List all knowledge bases |
| `ghl_kb_get` | Get a knowledge base |
| `ghl_kb_create` | Create a knowledge base |
| `ghl_kb_update` | Update a knowledge base |
| `ghl_kb_delete` | Delete a knowledge base |
| `ghl_kb_list_faqs` | List FAQs in a knowledge base |
| `ghl_kb_create_faq` | Add an FAQ |
| `ghl_kb_update_faq` | Update an FAQ |
| `ghl_kb_delete_faq` | Delete an FAQ |
| `ghl_kb_list_crawlers` | List web crawlers for a KB |
| `ghl_kb_get_crawler` | Get a crawler |
| `ghl_kb_create_crawler` | Add a web crawler |
| `ghl_kb_update_crawler` | Update a crawler |
| `ghl_kb_delete_crawler` | Delete a crawler |

## Workflows & Automation

| Tool | Description |
|------|-------------|
| `ghl_list_workflows` | List all workflows |
| `ghl_get_workflow` | Get a workflow |
| `ghl_create_workflow` | Create a workflow |
| `ghl_update_workflow` | Update a workflow |

## Action Notes

When creating conversation actions, the `actionParameters` must contain a top-level `name` and a `details` object. Common action types:
- `appointmentBooking` — books a calendar appointment
- `humanHandOver` — transfers to a human agent
- `stopBot` — stops the bot conversation
- `captureContactInfo` — captures contact fields
- `advancedFollowup` — triggers follow-up sequence
