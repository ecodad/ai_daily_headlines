# Agent Job Description: Reddit AI News Digest

**Agent Name:** AI News Digest Agent  
**Version:** 1.0  
**Owner:** Jonathan  
**Platform:** OpenClaw  
**Model:** gpt-oss-20b (LM Studio / local)

---

## Purpose

Monitor a defined set of AI-focused subreddits, aggregate posts by topic, and deliver a structured daily briefing. Notify via Discord three times daily with the top headlines. Maintain a persistent daily record and respond to on-demand requests for full briefing detail.

---

## Schedule

Runs three times per day at configurable intervals. Schedule may be adjusted without changes to core logic.

---

## Inputs

| Input | Detail |
|---|---|
| Subreddits | r/ClaudeAI, r/OpenAI, r/LocalLLaMA, r/clawdbot, r/artificial, r/MachineLearning, r/singularity (configurable) |
| Lookback window | 48 hours per run |
| Post filter | Minimum upvote threshold (configurable, suggested default: 50) |
| Deduplication source | Prior `.md` files and metadata from working directory |

---

## Workflow

1. **Fetch** — Pull posts from configured subreddits within the 48-hour window using the OpenClaw `reddit-readonly` skill. No Reddit API credentials required.
2. **Filter** — Remove posts below the upvote threshold. Remove posts already covered unless sentiment has changed.
3. **Cluster** — Group posts by topic. If multiple threads cover the same subject, treat as a single topic with aggregated stats.
4. **Rank** — Order topics by combined engagement (upvotes + comment count + thread count).
5. **Sentiment check** — Compare topic sentiment against prior metadata. Flag topics where overall sentiment has shifted since last coverage.
6. **Compose briefing** — Write daily `.md` file and update metadata file.
7. **Notify** — Push top 3 topic headlines to Discord.

---

## Outputs

### Daily `.md` File

Stored in a designated local working directory. One file per calendar day. Filename format: `YYYY-MM-DD-ai-news.md`

File structure:

```
# AI News Digest — [Date]

**Run metadata:** [timestamp] | Subreddits: [list] | Posts reviewed: [n] | Topics identified: [n]

---

## [Topic Headline A]

**Sentiment:** Positive / Mixed / Negative / Shifted ⚠️  
**Threads:** [n] | **Combined upvotes:** [n] | **Comments:** [n]  
**Summary:** [2–3 sentence synthesis of the topic across threads]  
**Sources:** [Link 1] | [Link 2] | [Link 3]  
**Related images:** [Link 1] [Link 2]

---

## [Topic Headline B]
...
```

### Daily Metadata File

Stored alongside the `.md` file. Filename format: `YYYY-MM-DD-metadata.json`

Contains: topics covered, sentiment tags, upvote counts, thread IDs, and timestamps. Used by subsequent runs for deduplication and sentiment comparison.

---

## Discord Integration

### Scheduled Push (3x daily)

Sends a message to the configured channel containing:
- Run timestamp
- Top 3 topic headlines with sentiment indicator
- Brief stat line per headline (upvotes, comment count, number of threads)
- Prompt to request full briefing

Example format:
```
📰 AI News Digest — [Time]

1. [Headline A] ↑4.2k | 312 comments | 5 threads
2. [Headline B] ↑2.1k | 198 comments | 3 threads — ⚠️ Sentiment shifted
3. [Headline C] ↑1.8k | 144 comments | 2 threads

Reply /briefing for the full daily report.
```

### On-Demand Command

Responds to `/briefing` sent in the monitored Discord channel. Delivers the full contents of the current day's `.md` file via direct message to the requesting user.

---

## Sentiment Change Logic

The agent compares the current run's aggregate sentiment for a topic against the sentiment recorded in the most recent metadata file where that topic appeared. A topic is re-surfaced if:

- Sentiment tag has changed (e.g. Positive → Negative), **or**
- The topic has materially new developments (significant upvote or comment activity on new angles)

Topics with no meaningful change are suppressed to avoid repetition.

---

## Constraints and Guardrails

- **Read-only Reddit access** — uses OpenClaw `reddit-readonly` skill via public JSON endpoints. No API credentials, no posting, voting, or account interaction of any kind.
- **Local directory access** — scoped strictly to the designated working directory.
- **Discord access** — limited to the configured channel and DM delivery. No other server actions.
- **No external API calls** beyond Reddit public endpoints and Discord webhook.
- **Graceful failure** — if a Reddit fetch fails, log the error and continue with available data. Do not send a partial or empty Discord notification without flagging the failure.
- **Cost control** — LLM calls are batched per run. Summaries are generated once per topic per run, not per post.

---

## Success Criteria

| Criterion | Target |
|---|---|
| Relevant topics surfaced per run | 5–10 distinct topics |
| Duplicate headline rate | < 10% across consecutive runs (excluding sentiment shifts) |
| Discord notification delivery | Within 5 minutes of scheduled run time |
| `.md` file written per day | 1 file, updated each run |
| On-demand `/briefing` response time | < 30 seconds |
| Headline quality (manual review) | 2–3 headlines per day align with topics covered in the AI Daily Brief podcast headline segment or main episode — assessed manually by reviewer, no automation required |

---

## Open Questions / Future Enhancements

- Add keyword blocklist to suppress low-quality or off-topic posts.
- Configurable subreddit list via Discord command without redeployment.
- Weekly digest rollup summarising the week's top themes.

---

*Last updated: 2026-02-19 — v1.1: switched from Reddit API to OpenClaw reddit-readonly skill; updated subreddit list*
