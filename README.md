# SKILLS_Burased

Claude Code skills and agents for GS Battery. The agent team works as one company:
see [`COMPANY.md`](COMPANY.md) for the org chart, roles and rules.

## Company (agents)

| Path | What it does |
|---|---|
| `COMPANY.md` | Org chart, roles and company rules |
| `agents/executive.md` | Strategy, board, KPIs, executive communication |
| `agents/business.md` | Business cases, market research, operations, kaizen |
| `agents/sales.md` | Account research, call prep, outreach, sales assets |
| `agents/product-manager.md` | Phase 1 Requirements, PRD, roadmap, sprints |
| `agents/researcher.md` | Phase R Research (AI track) |
| `agents/designer.md` | Phase 2 Design (shadcn/ui components only) |
| `agents/db-engineer.md` | Phase 3 Database |
| `agents/tester.md` | Phase 4 TDD and Phase 6 Testing |
| `agents/developer.md` | Phase 5 Write Code |

## SOP (from Siam-GS-Battery/agent-skill)

| Path | What it does |
|---|---|
| `skills/sop/` | GS Battery end-to-end SDLC SOP; every project starts on the AI track; TDD with Vitest, Playwright, k6; frontend is shadcn/ui + CVA only |
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

## Slides (own)

| Path | What it does |
|---|---|
| `skills/mckinsey-slides/` | McKinsey/BCG decks: SCQ, Pyramid, action titles, ghost deck, then one fixed design system |

## Business division (from awesome-claude-corporate-skills, MIT)

| Agent | Skills | Source folder |
|---|---|---|
| `executive` | `board-meeting-prep`, `change-management`, `competitive-analysis`, `deep-research`, `executive-communication`, `internal-comms`, `knowledge-synthesis`, `kpi-dashboard`, `ma-due-diligence`, `risk-assessment`, `strategic-planning`, `task-management` | `01-executive-leadership` |
| `business` | `business-case-builder`, `file-organizer`, `incident-postmortem`, `kaizen`, `meeting-insights-analyzer`, `memory-management`, `process-optimization`, `project-status-report`, `resource-planning`, `sop-builder`, `vendor-management`, `market-research`, `unit-economics` | `07-operations`, `04-marketing`, `02-finance-accounting` |
| `sales` | `account-research`, `account-research-common-room`, `call-prep`, `call-prep-common-room`, `competitive-intelligence`, `compose-outreach`, `contact-research`, `create-an-asset`, `daily-briefing`, `draft-outreach`, `enrich-lead`, `lead-research-assistant`, `prospect-apollo`, `prospect-common-room`, `sequence-load`, `weekly-prep-brief` | `05-sales` |
| `product-manager` | `brainstorming`, `doc-coauthoring`, `feature-spec`, `prd-writer`, `product-launch-playbook`, `roadmap-builder`, `search-strategy`, `source-management`, `sprint-planning`, `user-research-synthesizer` | `09-product-management` |

Licence: [`licenses/awesome-claude-corporate-skills.LICENSE`](licenses/awesome-claude-corporate-skills.LICENSE).

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
