# HEARTBEAT: MOLLY MESSENGER

## Trigger

Invoked by EDDIE EDITOR (Step 4) with run instructions. Runs once per
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
  3. Extract themes ranked 1, 2, 3
  4. Format Discord message from template (AGENTS.md)
  5. Send to discord_channel via discord_bot
  6. Report COMPLETED to EDDIE EDITOR
     OR report FAILED (with error) if send fails
```

## Exit Conditions

- After Discord message is confirmed sent and COMPLETED is reported
- After FAILED is reported if Discord send fails

## Do Not

- Loop or re-run without being re-invoked by EDDIE EDITOR
- Write to any files
- Use hardcoded Discord config
- Send more than one message per invocation
