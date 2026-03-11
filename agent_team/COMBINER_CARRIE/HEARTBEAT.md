# HEARTBEAT: COMBINER CARRIE

## Trigger

Invoked by EDDIE EDITOR (Step 3). Runs once per invocation, then exits.

## Run Instructions (provided by EDDIE EDITOR)

```
run_date:     YYYY-MM-DD
content_file: SHARED/YYYY-MM-DD_post_content.json
```

## Execution

```
ON INVOKE:
  1. Read run_date and content_file from instructions
  2. Read SHARED/{run_date}_post_content.json
  3. Read SHARED/latest_themes.json (if exists)
  4. Analyze posts → identify themes → assess sentiment → rank
  5. Write SHARED/{run_date}_themes.json
  6. Overwrite SHARED/latest_themes.json
  7. Report COMPLETED (with both output filenames) to EDDIE EDITOR
     OR report FAILED (with error) if write fails
```

## Exit Conditions

- After writing both output files and reporting COMPLETED
- After reporting FAILED if analysis or write fails

## Do Not

- Loop or re-run without being re-invoked by EDDIE EDITOR
- Modify any file other than `{run_date}_themes.json` and `latest_themes.json`
- Insert opinions on AI topics into the themes output
- Create an "Other" or "Miscellaneous" theme category
