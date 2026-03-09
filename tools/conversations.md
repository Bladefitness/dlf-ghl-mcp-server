# Conversations

Full inbox and messaging management — SMS, email, Facebook, Instagram, Live Chat, and more.

## Tools

| Tool | Description |
|------|-------------|
| `ghl_search_conversations` | Search conversations by contact, channel, or status |
| `ghl_get_conversation` | Get a conversation by ID |
| `ghl_create_conversation` | Create a new conversation |
| `ghl_update_conversation` | Update conversation status, assignee, etc. |
| `ghl_delete_conversation` | Delete a conversation |
| `ghl_get_conversation_messages` | Get all messages in a conversation |
| `ghl_get_message` | Get a single message by ID |
| `ghl_send_message` | Send SMS, email, Facebook, Instagram, or Live Chat message |
| `ghl_add_inbound_message` | Simulate an inbound message (for testing) |
| `ghl_add_outbound_call` | Log an outbound call |
| `ghl_get_email_message` | Get full email message content |
| `ghl_update_message_status` | Mark message as read/unread |
| `ghl_add_message_attachments` | Add attachments to a message |
| `ghl_upload_message_attachment` | Upload a file as a message attachment |
| `ghl_cancel_scheduled_messages` | Cancel a scheduled message |
| `ghl_send_typing_indicator` | Send a typing indicator to a conversation |
| `ghl_export_messages` | Export conversation messages |
| `ghl_get_call_log` | Get a call log entry |
| `ghl_list_call_logs` | List call logs with filters |
| `ghl_get_recording` | Get a call recording |
| `ghl_get_transcription` | Get a call transcription |
| `ghl_download_transcription` | Download a transcription file |

## Channel Types

When sending with `ghl_send_message`, specify `type`:
- `SMS`
- `Email`
- `WhatsApp`
- `Facebook`
- `Instagram`
- `Live_Chat`
- `GMB` (Google My Business)
