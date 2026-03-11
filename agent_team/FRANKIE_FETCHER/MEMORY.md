# MEMORY: FRANKIE FETCHER

## API Notes

- Reddit's public JSON API requires no authentication for read access to
  public subreddits
- Recommended request pace: ~1 request per second to avoid rate limiting
- HTTP 429 response indicates rate limiting — wait 60 seconds and retry once
  before recording as ERROR
- The `stickied` field on a post indicates pinned mod posts — always filter these out
- Pinned announcements often have artificially high scores; excluding them
  keeps the data focused on organic community discussion

## Subreddit Notes

<!-- FRANKIE updates this section if subreddits are frequently failing or restricted -->

| Subreddit            | Known Issue                  | Last Noted  |
|----------------------|------------------------------|-------------|
| —                    | —                            | —           |
