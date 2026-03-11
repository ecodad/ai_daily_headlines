# HEARTBEAT: BILLY BRIEFER

## Trigger

Invoked by EDDIE EDITOR (Step 5) with run instructions. Runs once per
invocation, then exits.

## Run Instructions (provided by EDDIE EDITOR)

```
run_date:        YYYY-MM-DD
themes_file:     SHARED/YYYY-MM-DD_themes.json
discord_bot:     [bot name from EDDIE_EDITOR/USER.md]
discord_server:  [server name/ID from EDDIE_EDITOR/USER.md]
discord_channel: [channel name/ID from EDDIE_EDITOR/USER.md]
```

## Execution

```
ON INVOKE:
  1. Read all run instructions from EDDIE EDITOR
  2. Read SHARED/{run_date}_themes.json
  3. Extract all themes in rank order
  4. Format full brief message(s) from template (AGENTS.md)
  5. If single message ≤ 2000 chars:
       Send one message to discord_channel
     Else:
       Send Part 1 (themes 1–5) → confirm → Send Part 2 (themes 6–N)
  6. Report COMPLETED to EDDIE EDITOR
     OR report FAILED (with error) if any send fails
```

## Exit Conditions

- After all Discord messages confirmed sent and COMPLETED is reported
- After FAILED is reported if any Discord send fails

## Do Not

- Loop or re-run without being re-invoked by EDDIE EDITOR
- Write to any files
- Use hardcoded Discord config
- Skip any themes from the themes file
