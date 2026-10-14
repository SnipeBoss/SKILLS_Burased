# Agent Company

Our agent team works as one company. Every role is a Claude Code subagent in `agents/`,
and every rule comes from the `sop` skill. When this file and `sop` disagree, `sop` wins.

## Org chart

```
CEO (human: Burased)                     approves every phase gate and every merge
└── Orchestrator (main Claude session)   plans, routes work, owns the handover
    ├── product-manager    Phase 1  Requirements
    ├── researcher         Phase R  Research (AI track only)
    ├── designer           Phase 2  Design
    ├── db-engineer        Phase 3  Database
    ├── tester             Phase 4  TDD, Phase 6 Testing
    ├── developer          Phase 5  Write Code
    └── clean-code-reviewer         Review gate before every merge (read-only)
```

## Roles and the skills they use

| Agent | Owns | Skills |
|---|---|---|
| Orchestrator | Track choice (Step 0), task split, handover, phase gate status | `sop`, `small-prs`, `request-refactor-plan` |
| `product-manager` | MoSCoW list, user stories, acceptance criteria, DoR | `sop`, `batch-grill-me`, `ubiquitous-language` |
| `researcher` | Experiments, approach choice, acceptance metric | `sop` |
| `designer` | UI and API shape from acceptance criteria | `sop`, `design-an-interface` |
| `db-engineer` | ERD, schema doc, migrations | `sop` |
| `tester` | Failing tests first, then integration, E2E and UAT | `sop`, `qa` |
| `developer` | Code that turns the tests green | `sop`, `clean-code-writer`, `ponytail`, `small-prs` |
| `clean-code-reviewer` | Clean Code review of the diff; never edits | `clean-code-review`, `ponytail-review` |

## Company rules

1. **The CEO decides.** Agents propose; only the human approves a phase gate, a merge or a push.
2. **Stay in your lane.** An agent only produces its own phase's artefacts (`sop` Multi-Agent rules).
3. **One branch per task.** Never commit to `main`, and never let two agents write the same file.
4. **Handover in writing.** Each task names its scope, input files, gate status and branch.
5. **Small PRs.** Each PR is one self-contained change (`small-prs`), reviewed before merge.
6. **No secrets in git.** Credentials stay in env, never in a prompt, file or commit.
