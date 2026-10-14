# SKILLS_Burased

Claude Code skills and agents for GS Battery. The agent team works as one company:
see [`COMPANY.md`](COMPANY.md) for the org chart, roles and rules.

## Company (agents)

| Path | What it does |
|---|---|
| `COMPANY.md` | Org chart: CEO (human), Orchestrator, one agent per SDLC phase |
| `agents/product-manager.md` | Phase 1 Requirements |
| `agents/researcher.md` | Phase R Research (AI track) |
| `agents/designer.md` | Phase 2 Design |
| `agents/db-engineer.md` | Phase 3 Database |
| `agents/tester.md` | Phase 4 TDD and Phase 6 Testing |
| `agents/developer.md` | Phase 5 Write Code |

## SOP (from Siam-GS-Battery/agent-skill)

| Path | What it does |
|---|---|
| `skills/sop/` | GS Battery end-to-end SDLC SOP, web-dev and AI tracks; the rules every agent follows |
| `docs/setup-guide/` | Thai setup guide for the agent skill |

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
