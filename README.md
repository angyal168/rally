# /rally -- Multi-Agent Coordination for Claude Code

> Coordinate multiple Claude Code agents through a shared bus file.

When you have 2+ Claude Code terminals open on the same project, `/rally` keeps them in sync. Each agent reads and writes to a shared markdown ledger so parallel work doesn't collide.

## Install

```bash
mkdir -p ~/.claude/commands
cp rally.md ~/.claude/commands/
```

Then type `/rally` in any Claude Code session.

## How It Works

1. First agent creates a rally file in `/agent-bus/` with a mission name
2. Other agents join by running `/rally` and selecting the active rally
3. Each agent reads the bus file before major actions and logs what they did after
4. Conflicts are flagged before they happen


<!-- forge-usage:v1 -->

## What it actually does

`/rally` coordinates several Claude Code agents that are open in separate terminals on the
same project, through one shared markdown file in `/agent-bus/`. There is no daemon, no
socket, and no server — the coordination substrate is a file every agent can read and append
to, which is why it works across terminals, machines and restarts.

The first agent to run `/rally` names the mission and creates
`/agent-bus/YYYY-MM-DD_<mission-name>.md` with a Roster table and a Log. Every later agent
that runs `/rally` finds the day's rallies, joins one, reads it for context on what the
others are doing, adds itself to the Roster, and appends a join entry.

## The standing instruction it installs

Joining is not the useful part — staying in sync is. After joining, each agent carries this
for the rest of its session: **read the rally file before any major action, append a log
entry after completing significant work.** Entries are capped at 2–4 lines per field so the
ledger stays readable.

```markdown
### Agent N | HH:MM
**Focus:** <current task>
**Status:** <what just got done or is in progress>
**Decisions:** <choices that affect other agents>
**Needs from others:** <specific asks, tagged by agent number>
**Blockers:** <anything stalled>
```

## The rules that keep it from rotting

- **Append only.** Never delete or overwrite another agent's entries — the file is a ledger.
- If a "Needs from others" entry is directed at you, address it before continuing your own work.
- If two agents are about to touch the same file, flag the conflict in the log *before* proceeding.
- Keep the file under 500 lines; summarise older entries into a `## Summary` section under
  the Roster when it gets long.

That third rule is the one that earns the whole thing: the common failure of parallel agents
is two of them editing the same file with no idea the other exists.

## Usage

```bash
mkdir -p ~/.claude/commands
cp rally.md ~/.claude/commands/
```

Run `/rally` in the first terminal, name the mission, then run `/rally` in each of the others
and join it.

## When not to use it

- Single-agent work. The bus is overhead with nothing to coordinate.
- Agents working on genuinely unrelated projects — a shared ledger of unrelated work is noise.
- As a lock. Rally makes conflicts *visible*; it does not prevent two agents writing the same
  file. Mutual exclusion takes an actual lock, which this is not.

## Requirements

Claude Code with a `~/.claude/commands/` directory, and a writable `/agent-bus/` path that
every participating agent can reach.

<!-- /forge-usage:v1 -->


<!-- forge-siblings:v1 -->

## More from the same author

Other free, open-source Claude Code tools in this family. Each one stands
alone -- none of them depend on this repo, or on each other.

- [smelt](https://github.com/angyal168/smelt) -- Extract actionable insights from any resource -- burn off the slag, keep the pure metal
- [dar](https://github.com/angyal168/dar) -- Lightweight audit trail for Claude Code -- Discovery, Artifact, Receipt
- [ralph](https://github.com/angyal168/ralph) -- Autonomous iteration loop for Claude Code -- define task, set condition, let it run
- [serious](https://github.com/angyal168/serious) -- Precision mode for Claude Code -- no hype, no ambiguity, only what's true
- [forge-prompt](https://github.com/angyal168/forge-prompt) -- Prompt coaching for Claude Code -- rates, sharpens, and rewrites your prompts into action-first form
- [council](https://github.com/angyal168/council) -- AI advisory board for Claude Code -- 6 executive perspectives debate any decision
- [logos-protocol](https://github.com/angyal168/logos-protocol) -- Forge an AI that knows you, remembers, and ascends. Open source, free, yours to imprint

<!-- /forge-siblings:v1 -->

## Part Of

This command is part of the [Logos Protocol](https://github.com/angyal168/logos-protocol) -- an open protocol for building an AI assistant that actually knows you.

## License

MIT

<!-- forge-related:v1 -->

## Related

This repo is one module. It handles keeping parallel agents coordinated; it does not compose itself into a working system -- that wiring is a separate job.

- **[The Forge Full Stack Bundle for Claude Code](https://notes.aingyal.com/go/gh-rally/nlajnm/)** -- a paid pack of Claude Code commands from the same author ($129).
- [All tools, free and paid](https://tools.aingyal.com/?utm_source=github&utm_medium=readme&utm_campaign=rally) -- the full index.

Listed so you can find them if they are useful to you. Nothing here is required to use this repo, which stays free.

<!-- /forge-related:v1 -->
