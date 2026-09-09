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

Then **fix the absolute path**: `claude/skills/agent-handoff/SKILL.md` and the
CLAUDE.md snippet both hardcode `C:\Users\John\.codex\skills\agent-handoff\SKILL.md`.
If the new machine's user profile is not `C:\Users\John`, rewrite both occurrences to
the real path, or the entry point will stop and report the shared spec as missing —
which is the intended failure mode, not a bug.

Verify after install: start Claude Code and say `接續 Codex 的任務`. The skill should
fire and the transcript should show it reading the `.codex` spec file before doing
anything else.

## Related, but not part of this repo

- Plugin `codex@openai-codex` (`/codex:transfer`, `/codex:rescue`, `/codex:review`, …).
  The entry point references `/codex:transfer` as a **user-invoked** optional session
  import. Install it separately with `/plugin`; the handoff workflow must work without it.
- `~/.codex/AGENTS.md` — broader personal Codex working agreements, not required here.
