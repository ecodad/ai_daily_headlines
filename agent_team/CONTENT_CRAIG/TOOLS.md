# TOOLS: CONTENT CRAIG

## Available Tools

### Web Fetch

- **http_get**
  Fetch a Reddit post with its comments.
  - URL: `https://www.reddit.com/r/{subreddit}/comments/{post_id}.json?limit=10&sort=top`
  - Required header: `User-Agent: Mozilla/5.0 (compatible; OpenClaw/1.0)`
  - Response: JSON array with two listing objects (post + comments)

### File Operations

- **read_file** — Read `SHARED/CONFIG.md` and `SHARED/{run_date}_post_metadata.json`
- **write_file** — Write `SHARED/{run_date}_post_content.json`

---

## Output JSON Schema

```json
{
  "fetch_date": "YYYY-MM-DD",
  "fetch_timestamp": "2026-03-10T14:10:00Z",
  "source_file": "2026-03-10_post_metadata.json",
  "total_posts_attempted": 74,
  "total_posts_ok": 71,
  "total_posts_error": 3,
  "posts": [
    {
      "post_id": "abc123",
      "subreddit": "r/MachineLearning",
      "rank_in_subreddit": 1,
      "title": "Post title here",
      "score": 1842,
      "num_comments": 134,
      "author": "username",
      "created_utc": 1741612800,
      "url": "https://www.reddit.com/r/MachineLearning/comments/abc123/post_title/",
      "permalink": "/r/MachineLearning/comments/abc123/post_title/",
      "is_self": true,
      "selftext": "Full post body text here. Empty string if link post.",
      "fetch_status": "OK",
      "top_comments": [
        {
          "author": "commenter1",
          "score": 234,
          "body": "Comment text here, truncated at 500 chars..."
        },
        {
          "author": "commenter2",
          "score": 89,
          "body": "Another comment..."
        }
      ]
    }
  ]
}
```
