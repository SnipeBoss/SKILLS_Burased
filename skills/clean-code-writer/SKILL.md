---
name: "clean-code-writer"
description: "Use when an agent writes, modifies, or refactors code and must follow Clean Code (Robert C. Martin) rules: naming, small functions, errors, tests, classes, boundaries."
---

# Clean Code Writer

Role: you are a coding agent that produces code a teammate can read once and understand. Code is read far more than written; optimise for the reader.

## Precedence (when rules conflict)
1. The user's explicit instruction.
2. The repository's existing conventions, linter and formatter config (ruff/black/eslint/prettier, CLAUDE.md, style guides).
3. Language idioms (PEP 8, Effective Go, etc.).
4. The rules in this skill.
5. Kent Beck's simple design order: tests pass > no duplication > expresses intent > fewest classes/methods.

Numeric thresholds below are review triggers, not hard laws. If you exceed one, say why in one line.

## Workflow
1. **Read before writing.** Open the files you will touch and their tests. Note naming style, error style, test framework, module layout.
2. **Pin behaviour.** If touching existing logic with no test, first add a characterization test that captures current behaviour.
3. **Write the failing test** for new behaviour (TDD loop: failing test -> minimal code -> refactor).
4. **Make it work**, simplest version.
5. **Make it clean**: run the self-check below; refactor in small steps, re-running tests each step.
6. **Boy Scout Rule**: leave touched code slightly cleaner (rename one vague name, extract one block). Do NOT refactor outside the task's footprint without asking.
7. **Run formatter, linter, tests.** Report the commands and results.
8. Keep refactor commits separate from behaviour-change commits.

## Rules

### Names
- Reveal intent: a name answers why it exists, what it does, how it is used. If it needs a comment, rename it.
- No disinformation (`account_list` must be a list), no near-identical names, no noise words (`data`, `info`, `obj`, `tmp`, `manager`, `result2`).
- Pronounceable and searchable. Replace magic numbers/strings with named constants.
- No encodings: no Hungarian notation, `m_`, or `I` prefixes.
- Classes/types are nouns; functions/methods are verbs; booleans read as predicates (`is_valid`, `has_items`).
- One word per concept across the codebase (do not mix fetch/get/retrieve); never one word for two concepts.
- Prefer solution-domain terms (Queue, Adapter, Repository); fall back to problem-domain terms.
- Name length scales with scope: short in tiny loops, descriptive when wide.
- Names declare side effects (`get_or_create_session`, not `get_session` that creates).

### Functions
- Small: aim for <= 20 lines; bodies of if/while/for ideally one call; nesting <= 2 levels.
- Do one thing. If you can extract a sub-function whose name is not a restatement, it does more than one thing.
- One level of abstraction per function. Order callers above callees (stepdown rule).
- Arguments: 0-2 ideal, 3 needs justification, 4+ -> group into an object/dataclass.
- No boolean flag arguments -> split into two functions.
- No output arguments; return values instead.
- No hidden side effects.
- Command-Query Separation: either change state or return information, not both.
- Replace long type-based if/elif/switch with polymorphism or a dispatch table, kept in one place (factory).
- DRY: extract duplicated logic; three similar blocks is the extraction trigger.

### Comments
- First try to express it in code (rename, extract).
- Allowed: why/intent, warnings of consequences, legal headers, public API docstrings, clarifying an opaque regex/format, TODO with owner or ticket.
- Forbidden: restating code, change logs/author tags (git does it), commented-out code (delete it), banner separators, closing-brace comments, misleading or stale comments.
- When you change code, update or delete the comments next to it.

### Formatting
- Newspaper order: high-level first, details below.
- Blank lines between concepts; related lines close together; declare variables near use.
- Follow the repo formatter. Never hand-align columns. Team style beats personal taste.

### Objects and data structures
- Objects hide data and expose behaviour; data structures (dataclass/DTO) expose data and hold no business logic. No hybrids.
- Do not auto-generate getters/setters for every field; expose abstract operations (`fuel_percent()` not `get_gallons()`).
- Law of Demeter: no train wrecks (`a.b().c().d()`); tell the object to do the work.

### Error handling
- Exceptions over return codes; keep the happy path unobstructed.
- Extract try/except bodies into their own function; error handling is one thing.
- Messages carry context: operation, input identifiers, cause.
- Define exception types by how the caller handles them; wrap third-party exceptions into one meaningful domain exception.
- Never `except: pass` or swallow silently. Never catch broad exceptions without re-raising or logging with context.
- Do not return None for collections (return empty); do not pass None as an argument; use Special Case / Null Object where it removes scattered checks.

### Boundaries (third-party code)
- Wrap external SDKs, drivers and raw dicts behind one adapter/module you own. Business logic never imports the vendor SDK directly.
- Write learning tests for unfamiliar libraries; they flag behaviour changes on upgrade.
- If the other side's API does not exist yet, define the interface you want and adapt later.

### Tests
- Every new branch/loop/parser/money/security path gets at least one test that fails if the logic breaks.
- Test code is production code: readable, DRY via helpers that read like domain language.
- Structure: Build-Operate-Check (Given-When-Then). One concept per test.
- F.I.R.S.T.: Fast, Independent, Repeatable (no real network, no wall-clock dependence; inject clocks/randomness), Self-validating, Timely.
- Test boundary conditions and the area around any bug you fix.

### Classes
- Order: constants, fields, public methods, private helpers under their caller.
- Single Responsibility: describe the class in one sentence without "and/or"; otherwise split.
- High cohesion: most methods use most fields. Falling cohesion -> extract a class.
- Open-Closed: extend by adding implementations, not by editing switch statements.
- Dependency Inversion: depend on abstractions; inject dependencies via constructor/params.

### Systems
- Separate construction from use: build and wire objects in `main`/composition root; no global clients inside business modules.
- Handle cross-cutting concerns (logging, retries, auth, transactions) with decorators/middleware, not inline everywhere.
- Grow architecture incrementally; avoid speculative frameworks and abstractions with one implementation.

### Concurrency
- Keep concurrency code separate from domain logic.
- Minimise shared mutable state; state owner and protection mechanism for each piece.
- Small critical sections; know the thread-safe primitives; use known patterns (producer-consumer, readers-writers).
- Every worker/loop has a clean shutdown path.
- Get single-threaded logic correct first. Treat any intermittent failure as a real bug.

## Self-check before handing back
Answer each; fix or justify:
- [ ] Any name that is vague, encoded, or needs a comment?
- [ ] Any magic number/string?
- [ ] Any function > ~20 lines, > 3 args, nesting > 2, or a boolean flag arg?
- [ ] Any function that both mutates state and returns a value?
- [ ] Any duplicated block?
- [ ] Any commented-out code, stale or redundant comment?
- [ ] Any `except: pass`, swallowed error, or None-returning collection?
- [ ] Any vendor SDK imported outside its adapter?
- [ ] Any attribute/getter chain longer than two dots?
- [ ] Any class whose purpose needs "and/or"?
- [ ] New logic covered by a test that would fail if it broke?
- [ ] Formatter, linter and tests run and green?

## Output format
1. The code change (diff or files).
2. Test results and commands run.
3. Up to 3 lines: rules intentionally bent and why; follow-ups outside scope (proposed, not done).