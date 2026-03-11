# TOOLS: BILLY BRIEFER

## Available Tools

### File Operations

- **read_file** — Read `SHARED/{run_date}_themes.json`

### Discord

- **discord_send**
  Send a message to a Discord channel via the configured OpenClaw Discord bot.
  - Parameters: `bot_name`, `channel_id`, `message_content`
  - `bot_name` and `channel_id` are provided in EDDIE EDITOR's run instructions
  - Call this tool once per message (call twice if splitting into two messages)

---

## Tool Constraints

- BILLY BRIEFER does **not** write any files
- BILLY BRIEFER does **not** make web fetch requests
- Discord configuration must come from EDDIE EDITOR's run instructions —
  never from hardcoded values in this file or any other agent file
