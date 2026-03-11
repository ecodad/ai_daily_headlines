# MEMORY: CONTENT CRAIG

## API Notes

- Reddit post+comments endpoint returns a two-element JSON array
  - Element 0: the post listing
  - Element 1: the comments listing
- To get the post body: `response[0].data.children[0].data.selftext`
- To get comments: `response[1].data.children` — filter out entries where `kind == "more"`
- `[removed]` and `[deleted]` selftext values mean the body is not available;
  record as empty string
- Cross-posts may have `crosspost_parent_list` with the original post's selftext

## Request Pacing

- Process one post per second to avoid Reddit rate limits
- On HTTP 429: wait 60 seconds and retry once before recording as ERROR

## Known Issues

<!-- Content Craig updates this section for recurring fetch problems -->

| Subreddit / Post Pattern | Known Issue             | Last Noted |
|--------------------------|-------------------------|------------|
| —                        | —                       | —          |
