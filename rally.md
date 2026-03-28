Rally -- multi-agent coordination through a shared bus file.

Use this when multiple Claude Code agents are open in separate terminals working on the same project. Rally creates or joins a shared markdown file in `/agent-bus/` so all agents stay aware of each other's work.

## How it works

1. Check `/agent-bus/` for any existing rally files from today.

2. **If no rally files exist for today** (this agent is the first):
   - Ask the user: "What's the mission? Give me a short name for this rally." (e.g., "two-pane-export", "gmail-pipeline", "decor-site")
   - Create the file: `/agent-bus/YYYY-MM-DD_<mission-name>.md`
   - Write the header:

```markdown
# Rally: <mission-name>
**Created:** YYYY-MM-DD HH:MM
**Mission:** <one-line description from user>

---

## Roster
| Agent | Focus | Joined |
|-------|-------|--------|
| Agent 1 | <this agent's current task> | HH:MM |

---

## Log

### Agent 1 | HH:MM
**Focus:** <what this agent is working on>
**Status:** Starting
**Decisions:** None yet
**Needs from others:** None yet
**Blockers:** None
```

   - Confirm: "Rally started. Tell other agents to run `/rally` to join."

3. **If rally files exist for today**, list them and ask:
   - "Found these active rallies: [list]. Which one are you joining?"
   - After confirmation, read the file to get context on what other agents are doing.
   - Add this agent to the Roster table with the next agent number.
   - Append a join entry to the Log.

4. **After joining or creating**, inject this standing instruction for the session:

> **RALLY PROTOCOL ACTIVE:** Before starting any major action, read the rally file at `/agent-bus/<filename>`. After completing a significant piece of work, append a log entry with your agent number, focus, status, decisions made, needs from others, and blockers. Keep entries concise -- 2-4 lines max per field. This keeps all agents in sync without overloading the file.

5. Remind the user: "I'll read the rally file before major actions and update it after completing work. Run `/rally` in your other agent windows to connect them."

## Log entry format

When appending to the rally file mid-session, use this format:

```markdown
### Agent N | HH:MM
**Focus:** <current task>
**Status:** <what just got done or what's in progress>
**Decisions:** <any choices made that affect other agents>
**Needs from others:** <specific asks, tagged by agent number if known>
**Blockers:** <anything stalled>
```

## Rules

- Never delete or overwrite another agent's log entries.
- Append only. The file is a shared ledger.
- If you see a "Needs from others" entry directed at you, address it before continuing your own work.
- If two agents are about to touch the same file, flag the conflict in the log before proceeding.
- Keep the rally file under 500 lines. If it's getting long, summarize older entries into a "## Summary" section at the top, below the Roster.
