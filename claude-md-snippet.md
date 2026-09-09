Append to `~/.claude/CLAUDE.md`. Adjust the absolute path if the user profile is not
`C:\Users\John`.

```markdown
# User rules

## Cross-agent handoff (Codex <-> Claude Code)

Codex and Claude Code take turns on the same tasks on this machine. Both implement.

When a request transfers task ownership either way — e.g. "switch to Codex",
"resume the Codex task", "我要換到 Codex", "接續 Codex 的任務" — use the
`agent-handoff` skill (`~/.claude/skills/agent-handoff/SKILL.md`). That skill is only
an entry point: it requires reading the shared specification at
`C:\Users\John\.codex\skills\agent-handoff\SKILL.md` in full, every time, before
handing off or accepting. Do not work from a summary of it, and do not edit or
duplicate it.

Sequential turns on this machine reuse the same worktree with uncommitted work left
intact. Keep one handoff/WORKLOG record per task; never start a second status system.
Wait until the previous agent has released the working directory before writing.
```
