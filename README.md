# SKILLS_Burased

Claude Code skills: Clean Code (Robert C. Martin), plus the third-party skills
used in the auto-predelivery monorepo.

## Clean Code (own)

| Path | What it does |
|---|---|
| `skills/clean-code-writer/` | Rules an agent follows when it writes or refactors code |
| `skills/clean-code-reviewer/` | Read-only review that reports smells by code (C/E/F/G/N/T) with severity and fix |
| `skills/clean-code-review/` | `/clean-code-review` command; runs the review in the `clean-code-reviewer` agent |
| `agents/clean-code-reviewer.md` | Read-only reviewer agent used by `clean-code-review` |

## Small PRs (own, based on Google eng-practices)

| Path | What it does |
|---|---|
| `skills/small-prs/` | Split work into small, self-contained PRs; summary of Google's "Small CLs" (CC-BY 3.0) |

## Third-party (vendored copies, MIT)

| Skills | Source | Licence |
|---|---|---|
| `ponytail`, `ponytail-audit`, `ponytail-debt`, `ponytail-gain`, `ponytail-help`, `ponytail-review` | ponytail plugin v4.9.0 (DietrichGebert) | [`licenses/ponytail.LICENSE`](licenses/ponytail.LICENSE) |
| `batch-grill-me`, `design-an-interface`, `edit-article`, `obsidian-vault`, `qa`, `request-refactor-plan`, `resolving-merge-conflicts`, `ubiquitous-language`, `writing-great-skills` | [mattpocock/skills](https://github.com/mattpocock/skills) | [`licenses/mattpocock-skills.LICENSE`](licenses/mattpocock-skills.LICENSE) |

## Install

Copy into a project (or `~/.claude/` for every project):

```bash
cp -r skills/* .claude/skills/
cp agents/* .claude/agents/
```
