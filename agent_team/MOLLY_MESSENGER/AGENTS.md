# OPERATIONAL RULES: MOLLY MESSENGER

## Execution Flow

1. Read run instructions from EDDIE EDITOR:
   - `run_date`, `themes_file`, `discord_bot`, `discord_server`, `discord_channel`
2. Read `SHARED/{run_date}_themes.json`
3. Extract themes with `rank: 1`, `rank: 2`, `rank: 3`
4. Format the Discord message using the template below
5. Send to the specified Discord channel
6. Report COMPLETED to EDDIE EDITOR

---

## Discord Message Template

```
📊 **AI Trends — Quick Summary** · {run_date}

**#1 · {theme_name}**
{summary}
{sentiment_emoji} {SENTIMENT}{sentiment_change_note}
{contributing_posts} posts · {total_upvotes} upvotes · {total_comments} comments
🔗 {representative_post_url}

**#2 · {theme_name}**
{summary}
{sentiment_emoji} {SENTIMENT}{sentiment_change_note}
{contributing_posts} posts · {total_upvotes} upvotes · {total_comments} comments
🔗 {representative_post_url}

**#3 · {theme_name}**
{summary}
{sentiment_emoji} {SENTIMENT}{sentiment_change_note}
{contributing_posts} posts · {total_upvotes} upvotes · {total_comments} comments
🔗 {representative_post_url}
```

---

## Field Substitution Rules

| Placeholder              | Source field in themes JSON          |
|--------------------------|--------------------------------------|
| `{run_date}`             | `run_date`                           |
| `{theme_name}`           | `theme_name`                         |
| `{summary}`              | `summary`                            |
| `{contributing_posts}`   | `contributing_posts`                 |
| `{total_upvotes}`        | `total_upvotes`                      |
| `{total_comments}`       | `total_comments`                     |
| `{representative_post_url}` | `representative_post_url`         |

## Sentiment Emoji

| Sentiment  | Emoji |
|------------|-------|
| `positive` | 🟢    |
| `neutral`  | 🟡    |
| `negative` | 🔴    |

Sentiment label is rendered in ALL CAPS: `POSITIVE`, `NEUTRAL`, `NEGATIVE`.

## Sentiment Change Note

| Condition                      | Text appended after sentiment label         |
|--------------------------------|---------------------------------------------|
| `sentiment_changed == true`    | ` ⚠️ changed from {previous_sentiment}`    |
| `sentiment_changed == false`   | `` (nothing)                                |

---

## Character Limit Handling

If the formatted message exceeds 2000 characters:
- Shorten each `summary` field to one sentence
- Never truncate theme names, sentiment labels, or Reddit URLs

---

## File Permissions

- READ:  `SHARED/{run_date}_themes.json`
- WRITE: None
- DO NOT modify: Any files
