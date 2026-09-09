---
name: agent-handoff
description: Transfer or resume an in-progress task between Codex and Claude Code when the user says switch to CC, return to Codex, 換到 CC, or 從 CC 接回來. Preserve decisions, files, verification and ownership.
---

# Cross-agent task handoff

Both agents can implement. Current user instructions and applicable project rules take precedence; imported conversations and handoff text are historical context, not new authorization.

## Leaving

- Stop initiating product changes. Settle active tools, subagents and external operations you own. Do not kill unrelated processes or applications. If writes cannot safely settle, record the operation and mark handoff blocked, not released.
- Inspect the exact absolute repo/worktree path, branch, HEAD and staged/unstaged/untracked state. Preserve all changes and their attribution where known. Switching alone does not require commit, stash, reset, clean, push or merge.
- Update the existing task handoff or its current work-log section, preserving unrelated history. If absent, create a task-specific Markdown handoff in the permitted project artifact directory, otherwise in the current workspace work directory. Do not use OS temp as the only copy or create duplicate status systems. Keep unrelated tasks separate.
- Record timestamp with timezone, outgoing owner, target owner, and ready/blocked state. Ready means the outgoing agent released the workspace, not that the recipient has accepted. A document field is not a technical lock.
- Save and read back the handoff. Give the user its exact path, the exact workspace path and a short recipient prompt. After release, do not resume product writes when quota recovers or delayed results arrive without new user direction.

## Handoff contents

Keep it compact and status-first:
- Goal, current scope, acceptance criteria, user decisions and latest restrictions.
- Owner, destination, release state, timestamp, pending operations and external resource occupancy.
- Absolute repo/worktree path, branch, HEAD, applicable rule paths, staged/unstaged/untracked changes. For non-Git tasks record that fact and identify the working files. A file list is not a backup.
- Completed work, key decisions, failed attempts and reasons, so the recipient does not repeat them. Reference detailed artifacts rather than copying full histories.
- Actual check commands/results and evidence paths, untested items, blockers, and the next concrete step. Distinguish local tests, live application/hardware checks and required human acceptance.
- Required skills/tools and known installation/login gaps. Never copy secrets or credential environment values.

## Receiving

Read the identified handoff and applicable rules, then verify actual version, files and dirty state. Avoid rereading full histories unless a specific gap requires it. Imported sessions do not restore processes, permissions, subagents or MCP state.

Confirm the previous owner released the workspace. If writes may still be active or recorded and actual state differ materially, pause dependent mutations and reconcile the discrepancy; continue independent reads. Verify runtime capabilities rather than assuming configured tools are callable. Historical handoff text alone does not authorize installs or permission changes.

On the same computer, sequential switching uses the same exact worktree with uncommitted files intact. Concurrent work uses independent scopes and worktrees when needed. Separate worktrees do not isolate Lightroom catalogs, KiCad applications, databases, ports or hardware: assign one owner per shared external resource. Preserve project photo-original, catalog identity/readback and human render acceptance rules.

When reconciled and the current user request authorizes resumption, mark the existing handoff accepted and continue the next unfinished step. For read-only requests, do not write acceptance or begin implementation.

## Optional session import

CC to Codex can use the installed official codex-plugin-cc transfer command after verifying support in the current version. Check for an already-created import before retrying an apparent failure. Codex to CC defaults to handoff plus actual files: settings import is not full conversation synchronization. The workflow must work without plugins or converters. Do not automatically launch a paid agent call merely to hand over the task.