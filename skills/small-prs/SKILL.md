---
name: small-prs
description: Split work into small, self-contained PRs that a reviewer can actually review. Use when a PR or branch is too big to review (a mega PR), when planning how a feature lands as several PRs, or when deciding what belongs in the current PR versus a follow-up.
---

# Small PRs

Based on Google's engineering practices, "Small CLs" (summary and sources in [`references/google-small-cls.md`](references/google-small-cls.md)). A **small PR** is one **self-contained** change:

- It does **one thing**, usually one part of a feature, not the whole feature.
- Its tests are in the same PR.
- A reviewer can understand it from the PR, its description, the codebase and PRs already merged.
- `main` stays **green** and usable after it merges.
- A new API ships with a caller in the same PR, so nothing unused lands.

**Size:** about 100 changed lines is comfortable, and about 1000 is too many. File count matters as much as line count: 200 lines in one file reads fine, 200 lines across 50 files does not. When unsure, go smaller.

## Splitting a mega PR

Work through the steps in order. Each step ends on its completion criterion.

### 1. Inventory

Diff against the merge base with `main`: `git diff --stat origin/main...HEAD` and `git log --oneline origin/main..HEAD`. Give every changed file (or hunk, in a mixed file) exactly one **concern**:

- feature slice
- bug fix
- refactor (rename, move or extract, with no behaviour change)
- test-only (new tests for existing code, test helpers)
- config or deploy (compose, Dockerfile, env)
- docs
- generated (lockfiles, snapshots)
- deletion
- **stowaway**: an unrelated change that came along for the ride

Done when every changed path has exactly one concern, and the stowaways are listed by name.

### 2. Plan the split before touching git

Choose one cut per concern:

| Cut | Use when |
|---|---|
| **Refactor first** | Behaviour changes sit on top of renames or moves. Land the pure refactor first, so the next PR reads as behaviour only. |
| **Tests first** | Existing logic has no tests. A test-only PR pins the current behaviour before the change. |
| **By files** | Parts need different reviewers or ship on different schedules (schema or contract before its code, config apart from code). Submit in dependency order; both can be reviewed at the same time. |
| **Horizontal** | Layers can be decoupled with a shared contract or stub (API schema, interface). Each layer is one PR. |
| **Vertical** | Independent end-to-end slices: one user-visible capability per PR. |
| **Stack** | A later PR needs an earlier one that is not merged yet. Base the PR on the earlier branch and retarget it to `main` once the earlier one merges. |
| **Own PR or drop** | A stowaway. Give it its own PR, or revert it. |

Write the plan as a table: PR, concern, paths, base branch, depends on, approximate lines changed.

Done when:
- every path from step 1 appears in exactly one PR;
- the dependency order has no cycles;
- each PR is within the size guidance above, or carries a one-line reason it can't be.

Plan the order so that every PR leaves `main` **green**.

### 3. Cut the branches

Start each branch from `origin/main`, or from the PR below it in a stack. Then bring the work over:

- **Whole files**: `git checkout <mega-branch> -- <paths>`.
- **Commits that already match a concern**: `git cherry-pick <sha>`.
- **Files that mix concerns**: `git checkout -p <mega-branch> -- <path>`, picking hunk by hunk.
- **A file that only compiles with code from a later PR**: move the dependency into an earlier PR, or add a stub.

On every branch, run the repo's CI commands (its lint and test tasks).

Done when each branch is green on its own.

### 4. Prove nothing was lost

Compare the union of all the branches with the mega branch. For a stack, use `git diff <top-of-stack> <mega-branch>`. For parallel branches, merge them all into a scratch branch and diff that against the mega branch.

Done when the diff is empty, or every remaining line is listed and was dropped on purpose (for example, a reverted stowaway).

### 5. Open the PRs and retire the mega PR

- Each PR description:
  - states its one concern;
  - links the PRs it depends on;
  - gives its place in the plan table.
- Request reviewers per PR. For a split by files, tell each reviewer about the sibling PR.
- Close the mega PR with a comment linking every replacement. Or keep it as a tracking PR that is never merged.

Done when every row in the plan has an open PR, and the mega PR points to all of them.

## Rules

- **Refactors travel alone.** Keep moves and renames out of PRs that change behaviour. A small rename that's needed for the change itself can stay.
- **Tests travel with logic.** A PR that adds or changes logic carries its tests. Test-only work (coverage for existing code, test helpers, a new integration harness) can go first as its own PR.
- **Green after every merge.** Order interdependent PRs so each one, merged alone, builds and passes.
- **Large is acceptable** only for:
  - deleting whole files (review cost is about one line);
  - output of a fully trusted automated refactor (the reviewer checks the intent).
  Anything else that seems unsplittable: first look for a refactor-first PR that makes it splittable, then agree with the reviewer in advance, and test it extra hard.
- **Keep moving while PRs wait.** Stack the next PR on the one under review rather than waiting for approval.
- **Plan PR boundaries before coding new work.** Cutting a finished mega PR afterwards is the expensive path this skill exists to repair.
