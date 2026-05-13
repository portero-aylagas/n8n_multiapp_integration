# Multi-App Integration Lab

## Real-world justification

This workflow represents a simple support intake process for a small team that receives incoming requests through a Telegram bot. The people who benefit are support team members or operators who need a structured list of incoming messages instead of checking a chat manually.

The workflow solves the problem of manually copying Telegram messages into a tracking table. By sending each bot message automatically to Airtable, the team gets a visible record of each request with sender, chat ID, timestamp, message ID, and message text. This reduces missed requests and makes follow-up easier.

Telegram fits the source role because it is a fast and simple channel for receiving user messages. Airtable fits the destination role because it provides a lightweight database where incoming requests can be reviewed, filtered, and tracked.

## Technical summary

The integration pair is Telegram and Airtable. The workflow uses a Telegram Trigger node to receive incoming bot messages, an Edit Fields / Set node to normalize the nested Telegram JSON into flat fields, and an Airtable node to create a new record. The mapped fields are Message ID from `message.message_id`, Timestamp from `$now`, Sender from `message.from.username` with `message.from.first_name` as fallback, Chat ID from `message.chat.id`, and Message Text from `message.text`. The hardest part was making sure n8n evaluated the expressions instead of storing them as literal text. One possible extension would be adding an IF node to ignore empty messages or commands such as `/start`.