# Productivity agent for Claude Code

A Claude Code subagent that keeps you focused: it captures tasks, turns vague
goals into next actions, plans your day, and runs daily/weekly reviews.

## Use it
Open this repo in Claude Code, then:

| Command | What it does |
|---|---|
| `/today [context]` | Plan today: top 3, quick wins, first next action |
| `/next [energy/time]` | The single thing to do right now, with a time box |
| `/capture <stuff>` | Dump tasks/ideas; they become concrete next actions |
| `/review` or `/review week` | Close the loop on the day or week |

Or just say "use the productivity agent to …".

State lives in plain Markdown: `TASKS.md` (Today / Next / Waiting / Someday /
Done) and `JOURNAL.md` (dated plans and reviews). Commit them to keep history.

## Use it in every project
Copy the agent to your user folder:
`cp .claude/agents/productivity.md ~/.claude/agents/` (and the commands to
`~/.claude/commands/`).
