# USER PROFILE: Handler Configuration

This file is the single source of truth for notification delivery settings.
EDDIE EDITOR reads this file at runtime and passes the values to
MOLLY MESSENGER and BILLY BRIEFER. To change notification destination,
update only this file — no other agent files need to change.

---

## Handler

- **Name:** Jonathan
- **Email:** jmhunt@mit.edu

---

## Notification Configuration

### Primary Channel: Discord

| Setting          | Value                                              |
|------------------|----------------------------------------------------|
| Method           | Discord Bot                                        |
| Bot Name         | [UPDATE: Your Discord bot name as set in OpenClaw] |
| Server Name/ID   | [UPDATE: Your Discord server name or ID]           |
| Channel Name/ID  | [UPDATE: Your Discord channel name or ID]          |

### Notification Schedule

| Agent           | Sends                  | Message Count        |
|-----------------|------------------------|----------------------|
| MOLLY MESSENGER | Top 3 themes           | 1 message per run    |
| BILLY BRIEFER   | All themes (up to 10)  | 1-2 messages per run |

---

## Notification Preferences

- **Tone:** Factual only — no editorializing, no opinions
- **Include per theme:**
  - Theme name and rank
  - 1–2 sentence summary
  - Sentiment label (positive / neutral / negative)
  - Sentiment change flag if theme repeated from prior run
  - Combined post count, upvote count, and comment count
  - One link to a representative original Reddit post

---

## Update Instructions

1. Update the Discord table above with your actual bot/server/channel values
2. Save this file
3. Changes take effect on the next cron-triggered run
4. Do not add Discord config to any other agent file
