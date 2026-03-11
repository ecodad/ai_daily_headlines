# OPERATIONAL RULES: EDDIE EDITOR

## Startup Procedure

1. Read `SHARED/STATUS.md`
2. Read `SHARED/CONFIG.md`
3. Set TODAY = current date formatted as YYYY-MM-DD
4. Check if a run for TODAY is already in progress:
   - If STATUS shows a step FAILED for TODAY → resume from that step
   - If STATUS shows all 5 steps COMPLETED for TODAY → do nothing, exit
   - If no run for TODAY exists → begin fresh from Step 1

---

## Step Execution Rules

Before invoking any agent:
- Write to STATUS.md: step name, status = IN_PROGRESS, timestamp

After the agent reports completion:
- Write to STATUS.md: step name, status = COMPLETED, timestamp, output file(s)

After the agent reports failure:
- Write to STATUS.md: step name, status = FAILED, timestamp, error description
- **Halt immediately. Do not proceed to the next step.**

---

## Discord Config Passthrough (Steps 4 and 5)

Before invoking MOLLY MESSENGER (Step 4):
1. Read `EDDIE_EDITOR/USER.md`
2. Extract: Discord Bot Name, Server, Channel ID
3. Include these values explicitly in the run instructions passed to MOLLY MESSENGER

Before invoking BILLY BRIEFER (Step 5):
1. Same as above — re-read USER.md fresh each time
2. Include Discord config in run instructions passed to BILLY BRIEFER

Discord config must come from USER.md at runtime. Never cache or hardcode it.

---

## Run Instructions Format

When invoking CONTENT CRAIG, COMBINER CARRIE, MOLLY MESSENGER, or BILLY BRIEFER,
include in the run instructions:

```
run_date: YYYY-MM-DD
metadata_file: SHARED/YYYY-MM-DD_post_metadata.json
content_file:  SHARED/YYYY-MM-DD_post_content.json
themes_file:   SHARED/YYYY-MM-DD_themes.json
```

For MOLLY MESSENGER and BILLY BRIEFER, also include:
```
discord_bot:     [value from USER.md]
discord_server:  [value from USER.md]
discord_channel: [value from USER.md]
```

---

## STATUS.md Update Format

After each step, append a row to the Step Log table in STATUS.md:

| Run Date   | Step                 | Status    | Timestamp (ISO 8601)     | Output File                          |
|------------|----------------------|-----------|--------------------------|--------------------------------------|
| 2026-03-10 | FRANKIE FETCHER      | COMPLETED | 2026-03-10T14:05:00Z     | 2026-03-10_post_metadata.json        |

Also update the "Current Run" section with the last completed step and timestamp.

After all 5 steps complete, update "Last Completed Run" section.

---

## File Permissions

- READ:  SHARED/STATUS.md, SHARED/CONFIG.md, EDDIE_EDITOR/USER.md
- WRITE: SHARED/STATUS.md, EDDIE_EDITOR/MEMORY.md
- DO NOT modify: Any data files (metadata, content, themes, latest_themes)
