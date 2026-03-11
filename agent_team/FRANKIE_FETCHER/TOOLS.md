# TOOLS: FRANKIE FETCHER

## Available Tools

### Web Fetch

- **http_get**
  Perform a GET request to Reddit's public JSON API.
  - URL pattern: `https://www.reddit.com/r/{subreddit}/hot.json?limit=25`
  - Required header: `User-Agent: Mozilla/5.0 (compatible; OpenClaw/1.0)`
  - No authentication needed
  - Expected response: Reddit Listing JSON (200 OK)

### File Operations

- **read_file** — Read `SHARED/CONFIG.md` and `SHARED/SUBREDDITS.md`
- **write_file** — Write `SHARED/YYYY-MM-DD_post_metadata.json`

---

## Output JSON Schema

```json
{
  "fetch_date": "YYYY-MM-DD",
  "fetch_timestamp": "2026-03-10T14:00:00Z",
  "config_used": {
    "min_upvotes": 50,
    "min_comments": 5,
    "posts_per_subreddit": 10,
    "fetch_limit": 25,
    "sort_by": "hot"
  },
  "summary": {
    "total_fetched": 200,
    "total_qualifying": 74,
    "subreddits_processed": 8,
    "subreddits_failed": 0
  },
  "subreddits": {
    "r/MachineLearning": {
      "status": "OK",
      "posts_fetched": 25,
      "posts_qualifying": 10,
      "posts": [
        {
          "rank": 1,
          "post_id": "abc123",
          "title": "Post title here",
          "score": 1842,
          "num_comments": 134,
          "author": "username",
          "created_utc": 1741612800,
          "url": "https://www.reddit.com/r/MachineLearning/comments/abc123/post_title/",
          "permalink": "/r/MachineLearning/comments/abc123/post_title/",
          "is_self": true
        }
      ]
    },
    "r/artificial": {
      "status": "ERROR",
      "reason": "HTTP 429 Too Many Requests"
    }
  }
}
```
