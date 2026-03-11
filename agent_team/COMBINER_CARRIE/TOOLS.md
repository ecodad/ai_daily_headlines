# TOOLS: COMBINER CARRIE

## Available Tools

### File Operations

- **read_file** — Read `SHARED/{run_date}_post_content.json` and
  `SHARED/latest_themes.json`
- **write_file** — Write `SHARED/{run_date}_themes.json` and
  overwrite `SHARED/latest_themes.json`

---

## Output JSON Schema

```json
{
  "run_date": "YYYY-MM-DD",
  "run_timestamp": "2026-03-10T14:20:00Z",
  "source_file": "2026-03-10_post_content.json",
  "total_posts_analyzed": 71,
  "total_themes_identified": 9,
  "themes": [
    {
      "rank": 1,
      "theme_name": "GPT-5 Release Speculation",
      "description": "Community discussion around anticipated features and timeline of OpenAI's next model.",
      "summary": "Posts across multiple subreddits debate expected capabilities and release date of GPT-5, with most speculation pointing to mid-2026.",
      "sentiment": "positive",
      "sentiment_changed": false,
      "previous_sentiment": "positive",
      "source_subreddits": ["r/OpenAI", "r/ChatGPT", "r/artificial"],
      "contributing_posts": 8,
      "total_upvotes": 4520,
      "total_comments": 612,
      "engagement_score": 5744,
      "representative_post_url": "https://www.reddit.com/r/OpenAI/comments/abc123/post_title/",
      "representative_post_title": "Post title here"
    },
    {
      "rank": 2,
      "theme_name": "Open Source Model Releases",
      "description": "New open-weight model releases and community benchmarking results.",
      "summary": "Several new open-source models were released this week, with community benchmarks showing competitive performance against proprietary alternatives.",
      "sentiment": "positive",
      "sentiment_changed": true,
      "previous_sentiment": "neutral",
      "source_subreddits": ["r/LocalLLaMA", "r/MachineLearning"],
      "contributing_posts": 6,
      "total_upvotes": 3100,
      "total_comments": 445,
      "engagement_score": 3990,
      "representative_post_url": "https://www.reddit.com/r/LocalLLaMA/comments/def456/post_title/",
      "representative_post_title": "Post title here"
    }
  ]
}
```
