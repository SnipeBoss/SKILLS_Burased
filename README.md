# SKILLS_Burased

Claude Code skills for Clean Code (Robert C. Martin).

| Path | What it does |
|---|---|
| `skills/clean-code-writer/` | Rules an agent follows when it writes or refactors code |
| `skills/clean-code-reviewer/` | Read-only review that reports smells by code (C/E/F/G/N/T) with severity and fix |
| `skills/clean-code-review/` | `/clean-code-review` command; runs the review in the `clean-code-reviewer` agent |
| `agents/clean-code-reviewer.md` | Read-only reviewer agent used by `clean-code-review` |

## Install

Copy into a project (or `~/.claude/` for every project):

```bash
cp -r skills/* .claude/skills/
cp agents/* .claude/agents/
```
