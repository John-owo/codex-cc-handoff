# codex-cc-handoff

Cross-agent task handoff between **Codex** and **Claude Code** running on the same
machine. Both agents implement; this is not a review handoff.

The design is deliberately split in two:

| File | Role |
|---|---|
| `codex/skills/agent-handoff/SKILL.md` | **The authoritative shared spec.** Single source of truth for leaving / handoff contents / receiving / shared-resource ownership. Owned jointly by both agents. |
| `claude/skills/agent-handoff/SKILL.md` | **Claude Code entry point only.** Contains no handoff format of its own — it forces a full read of the shared spec every time, then adds Claude-Code-specific mechanics. |
| `codex/skills/agent-handoff/agents/openai.yaml` | Codex-side skill metadata (display name, default prompt, implicit invocation). |
| `claude-md-snippet.md` | The rule to paste into `~/.claude/CLAUDE.md` so Claude Code actually routes ownership transfers to the skill. |

**Why the split:** a second full copy of the rules would drift. The entry point is
explicitly forbidden from copying, reformatting or "syncing" the shared spec.

## Install

Target layout on the new machine:

```
~/.claude/skills/agent-handoff/SKILL.md
~/.codex/skills/agent-handoff/SKILL.md
~/.codex/skills/agent-handoff/agents/openai.yaml
~/.claude/CLAUDE.md        <- append the snippet
```

The Claude Code entry point resolves the Codex skill from `CODEX_HOME` when set,
otherwise from the current user's `USERPROFILE/.codex` on Windows or `HOME/.codex`
elsewhere. No account path needs editing. If the shared spec is missing, the entry
point stops and reports the resolved path.

Verify after install: start Claude Code and say `接續 Codex 的任務`. The skill should
fire and the transcript should show it reading the `.codex` spec file before doing
anything else.

## Related, but not part of this repo

- Plugin `codex@openai-codex` (`/codex:transfer`, `/codex:rescue`, `/codex:review`, …).
  The entry point references `/codex:transfer` as a **user-invoked** optional session
  import. Install it separately with `/plugin`; the handoff workflow must work without it.
- `~/.codex/AGENTS.md` — broader personal Codex working agreements, not required here.

When a task used an external worker such as agy, the handoff records its run directory,
status, artifacts and pending integration. The receiving agent verifies those records
against current files and processes before continuing; a handoff does not resume a
worker session or grant its permissions.
