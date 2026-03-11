# OPERATIONAL RULES: CONTENT CRAIG

## Execution Flow

1. Read run instructions from EDDIE EDITOR → extract `run_date`, `metadata_file`
2. Read `SHARED/CONFIG.md` → extract TOP_COMMENTS_PER_POST, COMMENT_BODY_MAX_CHARS,
   COMMENT_SORT
3. Read `SHARED/{run_date}_post_metadata.json`
4. Iterate over all subreddits in the metadata file.
   For each subreddit with `status: "OK"`, iterate its posts in rank order:

   a. Construct URL:
      `https://www.reddit.com/r/{subreddit}/comments/{post_id}.json?limit={TOP_COMMENTS_PER_POST}&sort={COMMENT_SORT}`
      with header `User-Agent: Mozilla/5.0 (compatible; OpenClaw/1.0)`

   b. If fetch fails → record `fetch_status: "ERROR"` for that post, continue

   c. Extract post body: `data[0].data.children[0].data.selftext`
      - If empty string or `"[removed]"` or `"[deleted]"` → record as `""`

   d. Extract top comments from `data[1].data.children`:
      - Skip entries where `kind == "more"`
      - For each remaining comment, extract: `author`, `score`, `body`
      - Truncate `body` at COMMENT_BODY_MAX_CHARS characters
      - Keep first TOP_COMMENTS_PER_POST comments after filtering

   e. Add post record to output with all metadata carried forward from
      the metadata file plus the new `selftext` and `top_comments` fields

5. Write `SHARED/{run_date}_post_content.json`
6. Report COMPLETED to EDDIE EDITOR

---

## File Permissions

- READ:  `SHARED/CONFIG.md`, `SHARED/{run_date}_post_metadata.json`
- WRITE: `SHARED/{run_date}_post_content.json` (new file, never overwrite)
- DO NOT modify: Any other files
