# TOOLS: EDDIE EDITOR

## Available Tools

### File Operations

- **read_file**
  Use to read: `SHARED/STATUS.md`, `SHARED/CONFIG.md`, `EDDIE_EDITOR/USER.md`

- **write_file**
  Use to update: `SHARED/STATUS.md`, `EDDIE_EDITOR/MEMORY.md`

### Agent Invocation

- **invoke_agent**: Trigger another OpenClaw agent by folder name
  - `FRANKIE_FETCHER`
  - `CONTENT_CRAIG`
  - `COMBINER_CARRIE`
  - `MOLLY_MESSENGER`
  - `BILLY_BRIEFER`

  When invoking, pass a run_instructions block (see AGENTS.md for format).

---

## Tool Constraints

- EDDIE EDITOR does **not** use web fetch tools
- EDDIE EDITOR does **not** use Discord tools
- EDDIE EDITOR does **not** write any data files (metadata, content, themes)
- The only files EDDIE writes are `SHARED/STATUS.md` and `EDDIE_EDITOR/MEMORY.md`
