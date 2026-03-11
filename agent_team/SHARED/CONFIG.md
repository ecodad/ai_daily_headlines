# Global Configuration

All agents read this file for shared settings. Only EDDIE EDITOR modifies
the STATUS.md; no agent modifies CONFIG.md.

---

## Post Fetch Settings (FRANKIE FETCHER)

| Setting               | Value | Description                                      |
|-----------------------|-------|--------------------------------------------------|
| POSTS_PER_SUBREDDIT   | 10    | Max qualifying posts to keep per subreddit       |
| FETCH_LIMIT           | 25    | Posts to request from Reddit before filtering    |
| MIN_UPVOTES           | 50    | Minimum score (upvotes) for a post to qualify    |
| MIN_COMMENTS          | 5     | Minimum comment count for a post to qualify      |
| SORT_BY               | hot   | Reddit sort method: hot, new, top                |

## Content Fetch Settings (CONTENT CRAIG)

| Setting               | Value | Description                                      |
|-----------------------|-------|--------------------------------------------------|
| TOP_COMMENTS_PER_POST | 10    | Max top-level comments to fetch per post         |
| COMMENT_BODY_MAX_CHARS| 500   | Truncate individual comment bodies at this length|
| COMMENT_SORT          | top   | Reddit comment sort: top, best, new              |

## Theme Settings (COMBINER CARRIE)

| Setting               | Value | Description                                      |
|-----------------------|-------|--------------------------------------------------|
| MAX_THEMES            | 10    | Maximum themes to output per run                 |
| MIN_POSTS_PER_THEME   | 2     | Min contributing posts to form a theme           |
| ENGAGEMENT_FORMULA    | upvotes + (comments * 2) | Theme ranking formula       |

## File Naming (all agents)

- Raw metadata:   YYYY-MM-DD_post_metadata.json
- Post content:   YYYY-MM-DD_post_content.json
- Themes output:  YYYY-MM-DD_themes.json
- Latest themes:  latest_themes.json  (overwritten each run — no date prefix)
- Run status:     STATUS.md           (overwritten each run — no date prefix)
