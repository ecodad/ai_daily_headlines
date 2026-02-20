# AI News Digest — Agent Prompts

**Version:** 2.1  
**Last updated:** 2026-02-20  
**Owner:** Jonathan

---

## Overview

As of v2.0, the full pipeline prompt is maintained in `ai-news-pipeline.md`. All cron jobs call that file directly. This file is retained as a reference summary only.

**Cron job message (all three scheduled runs):**

```
Read and execute the full pipeline defined in /home/node/.openclaw/workspace/ai-news/ai-news-pipeline.md
```

---

## Tool Summary (reference only — full detail in ai-news-pipeline.md)

| Tool | Type | Purpose |
|---|---|---|
| Tool 1: RedditFetcher | Reddit skill (no LLM) | Fetch top posts from all subreddits in subreddits-to-watch.md |
| Tool 2: Prior metadata read | File read (no LLM) | Load previous metadata JSON for sentiment comparison |
| Tool 3: Cluster + Brief | ONE LLM call | Cluster posts, detect sentiment shifts, write briefing markdown and metadata JSON in single response |
| Tool 4: DiscordNotifier | Discord (no LLM) | Post top 3 clusters; respond to /briefing command |

---

## Key Files

| File | Purpose |
|---|---|
| `ai-news-pipeline.md` | Master pipeline prompt — called by all cron jobs |
| `subreddits-to-watch.md` | Source of truth for subreddit list and filtering config |
| `YYYY-MM-DD-ai-news.md` | Daily briefing output |
| `YYYY-MM-DD-metadata.json` | Daily cluster JSON for sentiment deduplication |
