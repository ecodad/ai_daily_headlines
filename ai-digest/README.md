# AI Reddit Digest Agent — Setup Guide

A daily AI news briefing agent for OpenClaw that monitors AI-focused subreddits, clusters posts into topics, detects sentiment shifts, and delivers a short digest to your messaging channel.

---

## Prerequisites

- OpenClaw installed and running (see [amanaiproduct/openclaw-setup](https://github.com/amanaiproduct/openclaw-setup))
- Messaging channel connected (WhatsApp or Telegram)
- **reddit-readonly skill** installed from ClawHub: `clawhub.ai/buksan1950/reddit-readonly`

### Install the reddit-readonly skill

```bash
openclaw skills install clawhub:buksan1950/reddit-readonly
openclaw skills list   # confirm it appears
```

---

## File Placement

Copy all four `.md` files into your OpenClaw workspace directory (default: `~/clawd/`):

```bash
cp SOUL.md AGENTS.md USER.md ~/clawd/
```

If your workspace is in a different location:
```bash
WORKSPACE=$(openclaw config get agents.defaults.workspace)
cp SOUL.md AGENTS.md USER.md "$WORKSPACE/"
```

---

## Configuration

1. **Open `USER.md`** and update:
   - `Messaging channel` — WhatsApp or Telegram
   - `Schedule` — change from `08:00 ET` if preferred
   - `Subreddits to Monitor` — add or remove as needed

2. **Create the memory directory** if it does not exist:
   ```bash
   mkdir -p ~/clawd/memory
   ```

3. **Restart the gateway** to pick up the new workspace files:
   ```bash
   openclaw gateway restart
   ```

---

## First Run (Manual Test)

Before relying on the schedule, trigger a manual run to verify everything works:

Via messaging app:
> "Run the reddit digest now and show me the results."

Via CLI:
```bash
openclaw agent --local --agent main --message "Run the reddit digest now and show me the results."
```

Expected output: A formatted digest with 3 topic clusters. If you see a skill error, check that reddit-readonly is installed correctly.

---

## Scheduled Runs

OpenClaw supports scheduled tasks via the workspace. To confirm the schedule is active after setup, send:
> "Confirm the reddit digest is scheduled for 08:00 ET daily."

To change the schedule:
> "Change the reddit digest to run at 07:30 ET."

---

## Requesting an Extended Briefing

After receiving a digest, reply with:
- `"more"` — receive all clusters (up to 10)
- A topic name (e.g., `"GPT-5 speculation"`) — receive a deeper summary for that cluster

---

## Sentiment Shift Alerts

When the agent detects a topic's sentiment has changed from the prior run (e.g., community mood around a tool shifted from Positive to Negative), it flags the entry with ⚠️ in the digest. Prior run data is stored in `~/clawd/memory/reddit-digest-last.json`.

---

## Troubleshooting

| Problem | Fix |
|---|---|
| No digest received | Check `openclaw logs --lines 50` for errors |
| reddit-readonly skill error | Run `openclaw skills list` — reinstall if missing |
| Sentiment shift not detecting | Check that `memory/reddit-digest-last.json` exists after first run |
| Wrong subreddits | Edit `USER.md` and restart gateway |

---

## Files in This Package

| File | Purpose |
|---|---|
| `SOUL.md` | Agent personality and editorial rules |
| `AGENTS.md` | Operational rules, clustering logic, output format |
| `USER.md` | Your preferences, schedule, subreddit list |
| `README.md` | This file |
