# Technical Design: Reddit AI News Digest Agent

**Version:** 1.3  
**Owner:** Jonathan  
**Platform:** OpenClaw  
**Model:** Claude Sonnet 4.6  
**Last updated:** 2026-02-20

---

## Prerequisites

No Reddit API registration required. The OpenClaw `reddit-readonly` skill uses Reddit's public JSON endpoints and requires no credentials. This eliminates a significant setup dependency.

**Discord:** Bot token, target channel ID, and DM permissions must be confirmed before the Discord tool is configured. Bot is already paired and tested.

---

## Agent Architecture

Single agent with four discrete tools. A multi-agent approach is deferred as a future learning exercise — chaining agents through a 20B local model introduces compounding failure points that would complicate initial build and debugging.

---

## Tools

### Tool 1: RedditFetcher

**Type:** OpenClaw built-in skill (`reddit-readonly`)  
**Purpose:** Pull posts from configured subreddits within the 48-hour window  
**LLM involvement:** None — pure skill execution  

Behavior:
- Queries each subreddit using `top` sort with `48h` time filter
- Returns post title, URL, upvote count, comment count, subreddit, and permalink
- Filters out posts below the configured upvote threshold before passing to next tool
- Caps output at top 60 posts by upvote count to protect context window

---

### Tool 2: TopicClusterer

**Type:** LLM call  
**Purpose:** Group filtered posts into named topic clusters with aggregated stats  
**LLM involvement:** One call per run (the first of two total)

Input: list of up to 60 post titles with upvote and comment counts  
Output: JSON array of topic clusters

Prompt strategy: instruct the model to return only valid JSON, no preamble. Specify the exact schema. Constrain to a maximum of 10 topics to bound output size.

Expected output schema:
```json
[
  {
    "topic": "Short descriptive headline (max 8 words)",
    "sentiment": "Positive | Negative | Mixed",
    "post_count": 3,
    "combined_upvotes": 4200,
    "combined_comments": 312,
    "representative_titles": ["title1", "title2"],
    "top_links": ["https://reddit.com/r/..."]
  }
]
```

Fallback: if JSON output is malformed, a simple keyword-frequency grouping will be applied as a backup clustering method before proceeding.

---

### Tool 3: BriefingWriter

**Type:** LLM call + file I/O  
**Purpose:** Compare clusters against prior metadata, write daily `.md` file, update metadata JSON  
**LLM involvement:** One call per run (the second of two total)

Steps:
1. Read prior day's `YYYY-MM-DD-metadata.json` from working directory
2. Pass topic clusters and prior metadata to LLM for sentiment comparison and summary writing
3. Write updated `.md` briefing file
4. Write updated metadata JSON

Prompt strategy: provide clusters and prior metadata side by side. Ask the model to write a 2–3 sentence summary per topic and flag any sentiment shifts with a ⚠️ marker. Return structured markdown directly.

Sentiment shift logic: a topic is flagged if the sentiment tag has changed since it last appeared in metadata, or if it carries a note of material new development. Topics with no change and covered within the last 48 hours are suppressed.

---

### Tool 4: DiscordNotifier

**Type:** Discord API  
**Purpose:** Scheduled push notifications and on-demand `/briefing` command  
**LLM involvement:** None — formatting is templated

**Scheduled push** (3x daily): reads the top 3 topics from the current run's cluster output and sends a formatted message to the configured channel.

Message template:
```
📰 AI News Digest — [HH:MM]

1. [Headline A] ↑4.2k | 312 comments | 5 threads
2. [Headline B] ↑2.1k | 198 comments | 3 threads — ⚠️ Sentiment shifted
3. [Headline C] ↑1.8k | 144 comments | 2 threads

Reply /briefing for the full daily report.
```

**On-demand `/briefing` command:** agent monitors the configured channel. On receipt of `/briefing`, reads the current day's `.md` file and sends full contents via DM to the requesting user.

---

## File Structure

```
/working-directory/          ← mounted as Docker volume (e.g. C:\Users\Jonathan\ai-news\)
  YYYY-MM-DD-ai-news.md      ← daily briefing, updated each run
  YYYY-MM-DD-metadata.json   ← topic/sentiment state for deduplication
  logs/
    YYYY-MM-DD.log           ← per-run execution log with timestamps
```

Docker volume mount maps the Windows path to `/data` inside the container.

---

## LLM Call Strategy

Two LLM calls per run, intentionally minimal:

| Call | Tool | Input size | Purpose |
|---|---|---|---|
| 1 | TopicClusterer | Up to 60 post titles + stats | Cluster and tag sentiment |
| 2 | BriefingWriter | Clusters + prior metadata | Write summaries, flag shifts |

Posts are trimmed to title only (no body text) before being passed to the LLM to stay well within context window limits. If title count exceeds 60, posts are ranked by upvotes and the bottom posts are dropped.

---

## Configuration

All tunable parameters live in `config.yaml`. No code changes required to adjust behavior.

```yaml
subreddits:
  - ClaudeAI
  - OpenAI
  - LocalLLaMA
  - clawdbot
  - artificial
  - MachineLearning
  - singularity

upvote_threshold: 50
post_cap: 60
lookback_hours: 48
run_times: ["07:00", "13:00", "19:00"]

discord:
  channel_id: YOUR_CHANNEL_ID
  command: /briefing

working_directory: /data

llm:
  model: gpt-oss-20b
  base_url: http://host.docker.internal:1234/v1
```

Note: `host.docker.internal` is the Docker-for-Windows hostname that allows the container to reach LM Studio running on the Windows host. This must be verified as reachable during initial setup.

---

## Scheduling

OpenClaw's built-in scheduler triggers the agent at the three configured run times. Run times are defined in `config.yaml`.

**Known risk:** WSL2 on Windows can experience clock drift that affects scheduled task timing. Each run logs its actual execution timestamp so drift can be detected. If drift becomes significant, an NTP sync step can be added to the WSL2 startup routine.

---

## Known Risks and Mitigations

| Risk | Likelihood | Mitigation |
|---|---|---|
| Topic clustering returns malformed JSON | Medium | Fallback keyword-frequency clustering |
| Context window exceeded by large post list | Low | Hard cap at 60 posts, titles only |
| Sentiment comparison unreliable with 20B model | Medium | Simple before/after tag comparison; avoid asking model to infer from raw text |
| WSL2 clock drift affects scheduling | Low | Log actual execution timestamps per run |
| Reddit public endpoints change or rate-limit | Low | Graceful failure logging; retry on next scheduled run |
| Discord DM delivery fails | Low | Log failure; do not silently drop |

---

## Build Order

1. ✅ Install and verify `reddit-readonly` skill
2. ✅ Build and test TopicClusterer
3. ✅ Build and test BriefingWriter
4. ✅ Build and test DiscordNotifier — scheduled push and `/briefing` command
5. ✅ Wire up scheduler — three daily cron jobs (07:00, 13:00, 19:00 ET)
6. ✅ End-to-end test — confirmed full run via cron
7. ⏳ Baseline calibration — run for 3–5 days and manually compare headlines against AI Daily Brief podcast

---

## Future Enhancements

- Sentiment shift deduplication — compare prior metadata and suppress unchanged topics
- Upvote threshold filtering — discard posts below 50 upvotes before clustering
- Keyword blocklist to suppress low-quality or off-topic posts
- Configurable subreddit list via Discord command without redeployment
- Weekly digest rollup summarising the week's top themes
- Multi-agent refactor (Fetcher agent + Summarizer agent) as a deliberate Claw Camp learning exercise once the single-agent version is stable
