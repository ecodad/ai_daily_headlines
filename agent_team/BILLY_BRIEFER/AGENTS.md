# OPERATIONAL RULES: BILLY BRIEFER

## Execution Flow

1. Read run instructions from EDDIE EDITOR:
   - `run_date`, `themes_file`, `discord_bot`, `discord_server`, `discord_channel`
2. Read `SHARED/{run_date}_themes.json`
3. Extract all themes in rank order (up to 10)
4. Format the full brief using the template below
5. Check total character count:
   - If ≤ 2000 chars → send as a single message
   - If > 2000 chars → split at rank 5/6 boundary, send as two messages
6. Send message(s) to the specified Discord channel
7. Report COMPLETED to EDDIE EDITOR

---

## Discord Message Template (single message or Part 1)

```
📋 **AI Trends — Full Brief** · {run_date}
{total_themes_identified} themes · {total_posts_analyzed} posts analyzed

{sentiment_emoji_1} **#1 · {theme_name_1}**
{summary_1}
{sentiment_change_note_1}{contributing_posts_1} posts · {total_upvotes_1} upvotes · {total_comments_1} comments
🔗 {representative_post_url_1}

{sentiment_emoji_2} **#2 · {theme_name_2}**
...

[continue for all themes in the message]

——
_Subreddits: {comma-separated source subreddits across all themes}_
```

---

## Two-Message Split

If splitting is required, label messages as follows:

**Message 1 of 2:**
```
📋 **AI Trends — Full Brief** · {run_date} (1/2)
Themes #1–5

[themes 1 through 5 using per-theme block format]
```

**Message 2 of 2:**
```
📋 **AI Trends — Full Brief** · {run_date} (2/2)
Themes #6–{N}

[themes 6 through N using per-theme block format]

——
_Subreddits: {comma-separated source subreddits across all themes}_
```

---

## Per-Theme Block Format

```
{sentiment_emoji} **#{rank} · {theme_name}**
{summary}
{sentiment_change_note}{contributing_posts} posts · {total_upvotes} upvotes · {total_comments} comments
🔗 {representative_post_url}
```

---

## Sentiment Emoji

| Sentiment  | Emoji |
|------------|-------|
| `positive` | 🟢    |
| `neutral`  | 🟡    |
| `negative` | 🔴    |

## Sentiment Change Note

| Condition                      | Text prepended to stats line                |
|--------------------------------|---------------------------------------------|
| `sentiment_changed == true`    | `⚠️ Sentiment changed from {previous_sentiment}\n` |
| `sentiment_changed == false`   | `` (nothing)                                |

---

## Character Limit Handling

Priority when reducing size:
1. Split into two messages first (preferred over truncation)
2. If a single theme's summary is very long, shorten to one sentence
3. Never truncate: theme names, sentiment labels, stats line, Reddit URLs

---

## File Permissions

- READ:  `SHARED/{run_date}_themes.json`
- WRITE: None
- DO NOT modify: Any files
