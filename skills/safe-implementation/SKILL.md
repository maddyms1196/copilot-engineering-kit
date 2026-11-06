---
name: safe-implementation
description: Implement features, enhancements, refactors and multi-file code changes safely, with scoped edits, preserved contracts and incremental verification. Use for requests to add or change functionality, extend an API, restructure or rename code, migrate to a new interface or library version, or apply a change across several files.
argument-hint: "[change to make]"
---

# Safe Implementation

## 1. Understand before editing
- Determine the requested behavior, acceptance criteria, invariants and what must stay unchanged.
- Read the code that will change, its callers, its tests and one or two nearby examples of the same pattern. Reuse existing helpers, error types, logging, configuration and dependencies instead of adding new ones.
- For a changed signature, schema, file format, event or API, find every consumer before editing (symbol references plus a text search for dynamic or string-based use).
- Before writing new code, check in order whether the need is already met by existing code, the standard library or platform, or an installed dependency. Write the minimum that remains.
- For refactors and other behavior-preserving changes, first capture current behavior with existing tests, characterization tests or recorded outputs on representative and edge inputs, then compare after each step. Preserve observable quirks (rounding, ordering, truncation, error behavior) unless asked to change them.

## 2. Plan proportionally
- Trivial change: edit directly.
- Multi-file change: note the ordered steps and the check that follows each one. A typical order is types and interfaces → core implementation → callers and adapters → tests → documentation and configuration.
- For large or long-running work, also follow the complex-task-execution skill.

## 3. Edit
- Keep each edit focused and consistent with local style (naming, structure, error handling, comment density). Do not reformat untouched code. Every changed line should trace to the request.
- Do not add speculative options, configuration, wrappers or abstractions for single uses, or handling for impossible cases. Keep validation, security and data-loss protection.
- Preserve backward compatibility unless the request says otherwise. If a breaking change is necessary, make it explicit and update every caller in the same change.
- Handle edge cases deliberately: empty, null or missing values, boundaries, invalid input, concurrency, partial failure, large inputs, and character encoding or locale.
- Validate untrusted input at trust boundaries. Use parameterized queries, safe path handling and existing authorization checks. Never log secrets.
- For dependencies, persisted schemas, migrations, build, CI or security changes, use authorization already given for that scope. Ask before consequential actions that exceed it; distinguish writing a migration from running it against shared data.

## 4. Test as you go
- Add or update tests for the new behavior: the expected case, boundaries, invalid input and error paths. Follow the project's test framework and fixture conventions.
- After each meaningful step, run the narrowest relevant check (targeted test, type check, compiler, linter). Fix failures you caused before moving on.
- Finish with the broader suite that the project defines, in proportion to risk.

## 5. Final diff review
Check the whole diff for accidental or unrelated edits, debugging leftovers, commented-out code, hard-coded values that belong in configuration, missing error handling, secrets, and documentation or changelog entries the project normally updates.

## Report
What changed and why (files); compatibility notes; tests added; checks run and their results; assumptions; anything not executed or still open.
