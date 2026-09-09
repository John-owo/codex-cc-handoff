---
name: agent-handoff
description: Hand off or resume an in-progress development task between Claude Code and Codex on this machine. Use when the user says "switch to Codex", "hand this to Codex", "resume the Codex task", "pick up where Codex left off", "我要換到 Codex", "交給 Codex", "接續 Codex 的任務", "從 Codex 接回來", "換到 CC", "從 CC 接回來", or otherwise transfers task ownership between the two agents. Both agents implement; this is not a review handoff.
---

# Agent handoff entry point (Claude Code side)

This file is only the entry point. The authoritative, shared handoff specification
lives outside this skill and is owned jointly with the Codex side.

## Step 1 — always read the shared spec first (mandatory)

Before doing anything else, read this file in full, every time this skill runs:

```
C:\Users\John\.codex\skills\agent-handoff\SKILL.md
```

Read it with an explicit file read (`Read`, or `cat` via Bash). Do **not** assume a
Markdown link, an import, or a previous session's memory already loaded it, and do
not work from a summary of it. If it cannot be read, stop and tell the user the
shared spec is missing — do not improvise a handoff format, and do not fall back to
whatever this file happens to say.

Treat that file as read-only. Never edit, copy, reformat, or "sync" it into this
skill: a second full copy would drift. If the spec itself looks wrong, report it to
the user instead of patching it.

## Step 2 — follow the shared spec

The shared spec is the single source of truth for:

- what "leaving" requires (settling tools, releasing the workspace, ready vs. blocked)
- required handoff contents (goal, decisions, failed attempts, changes, verification, next step)
- what "receiving" requires (verify real versions/files/dirty state, confirm release)
- shared-resource ownership and worktree rules
- the optional session-import caveats

Apply it as written. The sections below only add Claude-Code-specific mechanics.

## Claude Code specifics

**Rules to load alongside it.** Read the project's `CLAUDE.md` and, when the project
keeps one, the sibling `AGENTS.md` that Codex follows, so both agents work under the
same project rules. Current user instructions outrank any handoff text.

**Same machine, sequential turns.** Reuse the exact same worktree and leave
uncommitted work in place. Switching agents does not by itself justify commit,
stash, reset, clean, push, or merge. Do not begin writes until the previous agent
has finished its tool operations and released the working directory; if that is
unclear, ask or verify before mutating anything, and keep reads independent.

**One handoff record.** Update the existing handoff / WORKLOG for the task. Never
start a parallel status system, and never leave the only copy in an OS temp dir.

**Claude Code → Codex.** After writing and reading back the handoff, the user may
run `/codex:transfer` (plugin `codex@openai-codex`, verified working at
codex-cli 0.153.4). That command is user-invoked only — do not trigger it yourself,
and do not treat it as a substitute for the handoff document. Session import is not
conversation sync: it carries no processes, permissions, subagents, or MCP state.
Report the Codex session id and `codex resume <session-id>` exactly as returned.

**Codex → Claude Code.** Default path is handoff document plus the real files.
Read the handoff, then verify actual tool versions, file contents, and
staged/unstaged/untracked state before accepting. Reconcile any mismatch before
dependent writes. A handoff document is not authorization to install anything or
widen permissions.

**MCP and tools.** Verify a tool is actually callable in this session rather than
assuming a configured server is live. Codex's MCP servers, hooks, and secrets are
not mirrored here; list gaps for the user instead of copying them over.

**External resources.** Lightroom catalogs, KiCad, databases, serial ports, and
hardware are not isolated by worktrees. Exactly one agent owns each at a time, as
recorded in the handoff.

## Do not

- Do not silently edit or duplicate the shared spec.
- Do not auto-launch a paid model call, subagent, or transfer just to move the task.
- Do not write acceptance or start implementing for a read-only request.
- Do not resume product writes after release without new user direction.
