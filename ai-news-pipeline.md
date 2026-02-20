# AI News Digest — Pipeline Prompt

**Version:** 1.1  
**Last updated:** 2026-02-20  
**Owner:** Jonathan  
**Called by:** OpenClaw cron jobs (Morning, Evening)

---

## Overview

This file is the single source of truth for the AI News Digest pipeline. All cron jobs call this file. Do not hardcode subreddits or prompts in the cron job configuration — read them from here.

This pipeline is designed to complete in **two LLM calls maximum** per run to stay within API rate limits.

---

## Step 1: Read Configuration

Read `/home/node/.openclaw/workspace/ai-news/subreddits-to-watch.md` to get the list of subreddits to query. Extract the subreddit names from the table. Apply these constraints:

- Minimum upvote threshold: **50** (discard posts below this)
- Maximum posts passed to clustering: **60** (ranked by upvotes, drop lowest if over cap)
- Sort: **top** posts from the last **48 hours**
- Limit per subreddit: **10 posts**

---

## Step 2: Fetch Reddit Posts (reddit skill — no LLM call)

Use the reddit skill to fetch posts from each subreddit. For each subreddit:

```
reddit posts [SUBREDDIT] --sort top --time day --limit 10
```

Collect all results. Discard posts with fewer than 50 upvotes. If total exceeds 60, keep the top 60 by upvote count. Preserve for each post: `title`, `score`, `comments`, `permalink`, `subreddit`.

If a subreddit fetch fails, continue with remaining subreddits. Do not abort the run.

---

## Step 3: Read Prior Metadata (file read — no LLM call)

Read the most recent `YYYY-MM-DD-metadata.json` from `/home/node/.openclaw/workspace/ai-news/news_output/`. Store it for use in Step 4. If no prior file exists, proceed without it.

---

## Step 4: Cluster, Compare, and Write Briefing (ONE LLM call)

This is the single LLM call for the run. Pass all of the following in one prompt:

- The full list of collected post titles, scores, comment counts, and permalinks
- The prior metadata JSON (if available)

Instruct the LLM to do all of the following in one response:

**4a — Cluster posts into topics.** Group posts by topic. For each cluster produce:

```json
{
  "topic": "Short descriptive headline (max 8 words)",
  "sentiment": "Positive or Negative or Mixed",
  "post_count": 0,
  "combined_upvotes": 0,
  "combined_comments": 0,
  "representative_titles": ["title1", "title2"],
  "top_links": ["https://reddit.com/r/..."]
}
```

Rules: maximum 10 clusters, sorted by `combined_upvotes` descending, at least 1 permalink per cluster in `top_links`, uncategorised posts go in "Other".

**4b — Detect sentiment shifts.** Compare each cluster's sentiment against the prior metadata. If sentiment has changed for a matching topic, mark it with a sentiment shift flag.

**4c — Write the briefing markdown.** Return the complete briefing in this exact format:

```
# AI News Digest — YYYY-MM-DD

**Run metadata:** [HH:MM ET] | Subreddits: [list] | Posts reviewed: [n] | Topics identified: [n]

---

## 🟢/🔴/🟡 [Topic Headline]

**Sentiment:** Positive / Negative / Mixed / Shifted ⚠️
**Threads:** [post_count] | **Combined upvotes:** [combined_upvotes] | **Comments:** [combined_comments]
**Summary:** [2-3 sentence synthesis. Do not editorialize. Report what was posted.]
**Sources:** [top_links as markdown links, pipe-separated]

---
```

Sentiment emoji: 🟢 Positive, 🔴 Negative, 🟡 Mixed, ⚠️ Shifted.

The LLM response must contain the complete briefing markdown followed by the complete metadata JSON array, separated by this exact delimiter on its own line: `---METADATA---`

---

## Step 5: Save Files (file write — no LLM call)

Parse the LLM response at the `---METADATA---` delimiter.

Write the briefing markdown to:
```
/home/node/.openclaw/workspace/ai-news/news_output/YYYY-MM-DD-ai-news.md
```

Write the metadata JSON to:
```
/home/node/.openclaw/workspace/ai-news/news_output/YYYY-MM-DD-metadata.json
```

Overwrite both files if they already exist from an earlier run today.

---

## Step 6: Post to Discord (Discord delivery — no LLM call)

Post the top 3 clusters to the Discord channel in this exact format:

```
📰 AI News Digest — [HH:MM ET]

1. [topic] ↑[combined_upvotes] | [combined_comments] comments | [post_count] threads
2. [topic] ↑[combined_upvotes] | [combined_comments] comments | [post_count] threads
3. [topic] ↑[combined_upvotes] | [combined_comments] comments | [post_count] threads

Reply /briefing for the full daily report.
```

Append ` — ⚠️ Sentiment shifted` to any line where sentiment changed. If Discord delivery fails, log and do not retry.

---

## On-Demand: /briefing Command

When a user sends `/briefing` in the Discord channel:

1. Read today's `/home/node/.openclaw/workspace/ai-news/news_output/YYYY-MM-DD-ai-news.md`
2. Post the full contents to the channel
3. If today's file does not exist, respond: "No briefing available yet for today. Next scheduled run at [next run time]."
