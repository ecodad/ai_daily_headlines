# OPERATIONAL RULES: FRANKIE FETCHER

## Execution Flow

1. Read `SHARED/CONFIG.md` → extract MIN_UPVOTES, MIN_COMMENTS,
   POSTS_PER_SUBREDDIT, FETCH_LIMIT
2. Read `SHARED/SUBREDDITS.md` → extract active subreddit list
   (skip lines beginning with # and blank lines)
3. Set output filename: `SHARED/{TODAY}_post_metadata.json`
   where TODAY is provided in run instructions from EDDIE EDITOR
4. Initialize output JSON structure (see TOOLS.md for schema)
5. For each subreddit in the active list:
   a. Fetch: `GET https://www.reddit.com/r/{subreddit}/hot.json?limit={FETCH_LIMIT}`
      with header `User-Agent: Mozilla/5.0 (compatible; OpenClaw/1.0)`
   b. If fetch fails (non-200, timeout, invalid JSON):
      → Record `{status: "ERROR", reason: "..."}` for that subreddit
      → Continue to next subreddit
   c. Parse `data.children` array
   d. For each post (`child.data`), extract:
      - `id`, `title`, `score`, `num_comments`, `author`, `created_utc`,
        `url`, `permalink`, `is_self`
   e. Filter: discard posts where `score < MIN_UPVOTES`
              OR `num_comments < MIN_COMMENTS`
              OR `stickied == true`
   f. Sort remaining posts by `score` descending
   g. Keep the top POSTS_PER_SUBREDDIT posts
   h. Assign `rank` 1 through N (1 = highest score)
6. Write all results to `SHARED/{TODAY}_post_metadata.json`
7. Update summary section with totals
8. Report completion to EDDIE EDITOR

---

## Output Summary Section (required)

The output JSON must include a top-level summary:
```json
"summary": {
  "total_fetched": <int>,
  "total_qualifying": <int>,
  "subreddits_processed": <int>,
  "subreddits_failed": <int>
}
```

---

## File Permissions

- READ:  `SHARED/CONFIG.md`, `SHARED/SUBREDDITS.md`
- WRITE: `SHARED/YYYY-MM-DD_post_metadata.json` (new file, never overwrite existing)
- DO NOT modify: Any other files
