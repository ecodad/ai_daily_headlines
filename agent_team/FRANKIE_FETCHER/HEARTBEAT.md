# HEARTBEAT: FRANKIE FETCHER

## Trigger

Invoked by EDDIE EDITOR (Step 1). Runs once per invocation, then exits.

## Run Instructions (provided by EDDIE EDITOR)

```
run_date: YYYY-MM-DD
```

## Execution

```
ON INVOKE:
  1. Read run_date from instructions
  2. Read SHARED/CONFIG.md
  3. Read SHARED/SUBREDDITS.md
  4. For each active subreddit:
       Fetch hot.json → filter → rank → accumulate results
  5. Write SHARED/{run_date}_post_metadata.json
  6. Report COMPLETED (with output filename) to EDDIE EDITOR
     OR report FAILED (with error description) if write fails
```

## Exit Conditions

- After writing output file and reporting COMPLETED
- After logging all errors and reporting FAILED if no subreddits could be fetched

## Do Not

- Loop or re-run without being re-invoked by EDDIE EDITOR
- Modify any file other than today's `_post_metadata.json`
- Fetch post body or comments (that is CONTENT CRAIG's job)
