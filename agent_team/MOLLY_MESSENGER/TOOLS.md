# TOOLS: MOLLY MESSENGER

## Available Tools

### File Operations

- **read_file** — Read `SHARED/{run_date}_themes.json`

### Discord

- **discord_send**
  Send a message to a Discord channel via the configured OpenClaw Discord bot.
  - Parameters: `bot_name`, `channel_id`, `message_content`
  - `bot_name` and `channel_id` are provided in EDDIE EDITOR's run instructions
  - `message_content` is the formatted string built from the template in AGENTS.md

---

## Tool Constraints

- MOLLY MESSENGER does **not** write any files
- MOLLY MESSENGER does **not** make web fetch requests
- Discord configuration must come from EDDIE EDITOR's run instructions —
  never from hardcoded values in this file or any other agent file
