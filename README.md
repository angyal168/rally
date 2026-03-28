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

## Part Of

This command is part of the [Logos Protocol](https://github.com/angyal168/logos-protocol) -- an open protocol for building an AI assistant that actually knows you.

## License

MIT
