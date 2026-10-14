# Google eng-practices: "Small CLs" (summary)

Source: https://google.github.io/eng-practices/review/developer/small-cls.html
Project: https://github.com/google/eng-practices (licence: CC-BY 3.0, as stated in the repository).
This is a paraphrased summary for agents. Read the source for the full text. "CL" (changelist) means a PR.

## Why small

- **Faster review.** Several five-minute reviews fit into a day more easily than one 30-minute block.
- **More thorough review.** Big changes draw long rounds of comments, and important points get lost in them.
- **Fewer bugs.** The author and the reviewer can both reason about a small change.
- **Less wasted work.** If the reviewer rejects the direction, less work is thrown away.
- **Easier to merge.** Big changes sit open longer, collect conflicts and need repeated rebases.
- **Easier to design well.** A small change is easier to polish.
- **Less blocking.** Self-contained pieces let the author keep working while earlier PRs wait for review.
- **Easier to roll back.** A big change is more likely to touch files that were changed again after it merged.
- **Reviewers may reject a PR for size alone.** Splitting it after the fact costs more than writing it small from the start.

## What counts as small

- **One self-contained change.**
  - It addresses one thing, usually part of a feature. When in doubt, err toward too small.
  - Its related tests are included.
  - The reviewer can understand it from the PR, its description, the codebase and earlier PRs.
  - The system still works for users and developers after it lands.
  - The impact is clear. A new API comes with a usage of it in the same PR.
- **Size guide:** about 100 lines is usually fine and about 1000 is usually too big. It is the reviewer's call. Spread matters too: 200 lines in one file can be fine, while 200 lines across 50 files usually is not.
- Authors underestimate how hard their own change is to read. Reviewers seldom complain that a PR is too small.

## When a large PR is acceptable

- **Deleting a whole file.** It reviews almost like a one-line change.
- **Output of a fully trusted automated refactoring tool.** The reviewer checks the intent rather than every line. Merge and test caveats still apply.

## Splitting strategies

- **Plan first.** Before writing code, decide how the work splits into PRs and how they depend on each other.
- **Stacking.** Send a PR for review, then start the next one on top of it right away.
- **By files.** Group the files that need different reviewers but form a self-contained change.
  - Example: a schema or protocol change in one PR and the code that uses it in another. The schema PR lands first; both can be reviewed at the same time; tell each set of reviewers about the other PR.
  - Example: code in one PR and its configuration or experiment in another. Rollback is easier because config often ships faster than code.
- **Horizontal.** Decouple layers (client, API, service, data model) with shared contracts or stubs, so each layer moves on its own. The same idea works at function and class level.
- **Vertical.** Split into thin full-stack features that progress in parallel, for example multiplication and division as separate PRs even though they share UI or validation code.
- **Horizontal and vertical together.** Build a grid: each cell (one layer of one feature) is a standalone PR, built from the model layer up to the client.

## Separating refactorings

- Refactorings (moves, renames) go in their own PR, separate from feature work and bug fixes. Mixing them makes both harder to understand.
- Tiny cleanups, such as renaming a local variable, can stay in the feature PR. The author and the reviewer decide when a refactoring is big enough to split out.

## Tests

- "Small" is about focus, not a line count.
- A PR that adds or changes logic includes new or updated tests.
- A pure refactoring is covered by tests: existing ones if possible, new ones if there are none.
- Test-only PRs can go first:
  - adding tests to existing code (this pins behaviour before a refactor);
  - refactoring test code, such as adding helpers;
  - adding larger test infrastructure, such as an integration harness.

## Don't break the build

When PRs depend on each other, the whole system must keep working after each one is submitted. A broken build blocks other developers.

## "It can't be made small enough"

- This is very rare. People who practise small PRs can almost always decompose the work.
- Look for a refactoring-only PR that would make a cleaner split possible, and ask teammates for ideas.
- As a last resort, get the reviewers' agreement in advance, expect a long review, watch closely for bugs and be especially thorough with tests.
