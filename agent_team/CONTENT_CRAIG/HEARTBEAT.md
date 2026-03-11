# HEARTBEAT: CONTENT CRAIG

## Trigger

Invoked by EDDIE EDITOR (Step 2). Runs once per invocation, then exits.

## Run Instructions (provided by EDDIE EDITOR)

```
run_date:      YYYY-MM-DD
metadata_file: SHARED/YYYY-MM-DD_post_metadata.json
```

## Execution

```
ON INVOKE:
  1. Read run_date and metadata_file from instructions
  2. Read SHARED/CONFIG.md
  3. Read metadata_file
  4. For each post in each qualifying subreddit:
       Fetch post body + top comments from Reddit JSON API
  5. Write SHARED/{run_date}_post_content.json
  6. Report COMPLETED (with output filename) to EDDIE EDITOR
     OR report FAILED (with error description) if write fails
```

## Exit Conditions

- After writing output file and reporting COMPLETED
- After logging all post errors and reporting FAILED if the write itself fails

## Do Not

- Loop or re-run without being re-invoked by EDDIE EDITOR
- Modify any file other than today's `_post_content.json`
- Attempt to analyze or summarize post content
