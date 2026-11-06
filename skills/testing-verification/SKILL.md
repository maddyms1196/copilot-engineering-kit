---
name: testing-verification
description: Design, write, run and interpret tests and other verification checks, and confirm that a change works without regressions. Use when adding or fixing tests, when tests fail, when asked to verify or validate a change, to check for regressions, to raise coverage of risky code, or before declaring a code or analysis task complete.
argument-hint: "[code, change, or failing test]"
---

# Testing and Verification

## 1. Discover how this project tests
- Find the framework, commands and conventions from the project itself: test directories and naming patterns, manifest scripts (for example the `scripts` in `package.json`, or tool sections in `pyproject.toml`), `Makefile` or task-runner targets, CI workflow steps, and contributor docs.
- Reuse existing fixtures, factories, helpers and mocking style. Do not introduce a new test framework or dependency without approval.
- If no test setup exists, say so and propose the lightest option consistent with the stack (for example the language's standard-library test runner).

## 2. Decide what to test
- Start from the behavior and invariants, not the implementation details.
- Cover the expected case, boundaries (empty, one, many, limits, off-by-one), invalid input, error paths, and the specific regression being fixed.
- For integration points, test the contract (inputs, outputs, error shapes) and prefer local fakes over network or shared services.
- For data code, check schema, row counts through each transformation, key uniqueness before and after joins, conservation of totals, null handling, and numeric tolerance instead of exact float equality.

## 3. Write good tests
- Make them deterministic and isolated: no reliance on test order, wall-clock time, unseeded randomness, real network, or shared mutable state. Inject or freeze time and seeds.
- Use one clear reason to fail per test and descriptive names. Assert on observable results, not on private internals.
- For a bug fix, confirm the new test fails without the fix when that is practical.

## 4. Run in widening circles
1. The single new or affected test.
2. The test file or module for the changed code.
3. Related suites (callers, integration tests).
4. The full suite, linters and type checks, when permitted and proportionate.
Record each command and its result. When a run is slow or costly, say which levels were skipped and why.

## 5. Interpret failures
Classify each failure as a regression from this change, a pre-existing failure (show evidence, such as it failing on unchanged code), an environment or setup problem, or flakiness (show repeated runs). Fix regressions you caused. Report the others without hiding them.

## Never
Loosen assertions, add skips, delete tests, catch and ignore errors, or update snapshots or expected outputs just to make a run pass. If an expectation is genuinely wrong, explain why and get agreement when the intent is unclear.

## Completion standard
Report which checks passed, failed or were not run (with the reason), what remains unverified, and the exact commands someone else can run to reproduce the results. "Looks correct" is not verification.
