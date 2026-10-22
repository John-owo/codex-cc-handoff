Append to `~/.claude/CLAUDE.md`.

```markdown
# User rules

## Cross-agent handoff (Codex <-> Claude Code)

Codex and Claude Code take turns on the same tasks on this machine. Both implement.

When a request transfers task ownership either way — e.g. "switch to Codex",
"resume the Codex task", "我要換到 Codex", "接續 Codex 的任務" — use the
`agent-handoff` skill (`~/.claude/skills/agent-handoff/SKILL.md`). That skill is only
an entry point: it resolves the installed shared specification from `CODEX_HOME`
when set, otherwise from the current user's `USERPROFILE/.codex` on Windows or
`HOME/.codex` elsewhere, then reads `skills/agent-handoff/SKILL.md` in full every
time before handing off or accepting. Do not work from a summary of it, and do not
edit or duplicate it. If the file is missing, report the resolved path and stop.

Sequential turns on this machine reuse the same worktree with uncommitted work left
intact. Keep one handoff/WORKLOG record per task; never start a second status system.
Wait until the previous agent has released the working directory before writing.
```
