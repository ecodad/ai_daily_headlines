# HEARTBEAT: EDDIE EDITOR

## Trigger

Invoked by external cron job. Runs once per invocation, then exits.
EDDIE does not self-schedule. All other agents are triggered by EDDIE.

---

## Execution Loop

```
ON INVOKE:
  1. Read SHARED/STATUS.md
  2. Read SHARED/CONFIG.md
  3. Set TODAY = YYYY-MM-DD (current date)
  4. Determine start step (see Startup Procedure in AGENTS.md)

  STEP 1 — FRANKIE FETCHER
    Write STATUS.md: Step 1 IN_PROGRESS
    Invoke FRANKIE_FETCHER with run_date = TODAY
    On COMPLETED → Write STATUS.md: Step 1 COMPLETED + output filename
    On FAILED    → Write STATUS.md: Step 1 FAILED + error → HALT

  STEP 2 — CONTENT CRAIG
    Write STATUS.md: Step 2 IN_PROGRESS
    Invoke CONTENT_CRAIG with run_date = TODAY, metadata_file path
    On COMPLETED → Write STATUS.md: Step 2 COMPLETED + output filename
    On FAILED    → Write STATUS.md: Step 2 FAILED + error → HALT

  STEP 3 — COMBINER CARRIE
    Write STATUS.md: Step 3 IN_PROGRESS
    Invoke COMBINER_CARRIE with run_date = TODAY, content_file path
    On COMPLETED → Write STATUS.md: Step 3 COMPLETED + output filename
    On FAILED    → Write STATUS.md: Step 3 FAILED + error → HALT

  STEP 4 — MOLLY MESSENGER
    Read USER.md → extract Discord config
    Write STATUS.md: Step 4 IN_PROGRESS
    Invoke MOLLY_MESSENGER with themes_file path + Discord config
    On COMPLETED → Write STATUS.md: Step 4 COMPLETED
    On FAILED    → Write STATUS.md: Step 4 FAILED + error → HALT

  STEP 5 — BILLY BRIEFER
    Read USER.md → extract Discord config (re-read fresh)
    Write STATUS.md: Step 5 IN_PROGRESS
    Invoke BILLY_BRIEFER with themes_file path + Discord config
    On COMPLETED → Write STATUS.md: Step 5 COMPLETED
    On FAILED    → Write STATUS.md: Step 5 FAILED + error → HALT

  ON FULL COMPLETION:
    Update STATUS.md: Last Completed Run = TODAY, all steps COMPLETED
    Append run record to EDDIE_EDITOR/MEMORY.md
    EXIT
```

---

## Exit Conditions

- All 5 steps completed successfully → normal exit
- Any step FAILED → halt and exit after writing STATUS.md
- Run for TODAY already fully completed → exit without action

## Do Not

- Run agents in parallel
- Skip steps even if output files already exist (except when resuming)
- Modify any data files (metadata, content, themes)
- Send Discord messages directly
