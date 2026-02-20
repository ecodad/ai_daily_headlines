# AI News Digest Agent

An automated AI news briefing system built on OpenClaw as part of the Claw Camp learning experience. The agent monitors seven AI-focused subreddits, clusters posts by topic, and delivers structured briefings to a private Discord channel twice daily.

## What It Does

- Fetches top posts from configured subreddits using the OpenClaw reddit skill
- Groups posts into topic clusters with sentiment tagging
- Writes a daily markdown briefing and metadata file
- Posts the top 3 topics to Discord on a scheduled basis
- Responds to `/briefing` in Discord with the full daily report

## Schedule

Runs twice daily via OpenClaw cron jobs. Times are configured in the OpenClaw dashboard with `America/New_York` timezone.

## Folder Structure

```
ai-news/
├── ai-news-pipeline.md       # Master pipeline prompt — called by all cron jobs
├── agent-prompts.md          # Tool summary and cron message reference
├── subreddits-to-watch.md    # Source of truth for subreddits queried and filter config
│
├── news_output/              # All pipeline run artifacts
│   ├── YYYY-MM-DD-ai-news.md       # Daily briefing (one per day, overwritten each run)
│   └── YYYY-MM-DD-metadata.json    # Topic cluster JSON used for sentiment comparison
│
└── development/              # Project documentation
    ├── reddit-ai-news-agent-jd.md      # Agent job description and success criteria
    ├── reddit-ai-news-tech-design.md   # Technical design document
    ├── session-notes-YYYY-MM-DD.md     # Build session notes
    └── archive/                        # Superseded prompt files retained for reference
        ├── briefing-prompt.md
        └── topic-clusterer-prompt.md
```

## Key Files

| File | Purpose |
|---|---|
| `ai-news-pipeline.md` | Single source of truth for pipeline logic. Edit this to change agent behavior. |
| `subreddits-to-watch.md` | Add or remove subreddits here. No other changes required. |
| `agent-prompts.md` | Reference summary of tools and cron message. Not called directly by the agent. |

## Model

Claude Sonnet 4.6 via Anthropic API. The pipeline is designed to complete in one LLM call per run to minimize API usage.

## Built With

- [OpenClaw](https://campclaw.ai) — AI agent framework
- Reddit public JSON endpoints via OpenClaw reddit skill (no API credentials required)
- Discord via OpenClaw bot integration
