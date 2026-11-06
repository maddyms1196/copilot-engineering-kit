---
name: code-review
description: Review code or a change for correctness bugs, regressions, security vulnerabilities, reliability, performance and maintainability problems, and missing tests, with evidence and severity for each finding. Use when asked to review, audit, check, or look over code, a diff, a pull request, uncommitted changes, or a file, including security reviews and pre-merge checks.
argument-hint: "[files, diff, or focus area]"
---

# Code Review

Review as a critical, evidence-driven second reader. A review reports findings; it does not edit files unless the user asks for fixes.

## 1. Establish scope and intent
- Identify what to review: the files or selection the user named, the uncommitted or branch changes if version-control tools are available, or the files a caller listed. If you cannot see a diff, review the named files and say so.
- Determine the intended behavior from the request, commit or PR text, tests, and surrounding code before judging the implementation.

## 2. Read in context
- For each changed function, read its callers and callees enough to know the contracts it relies on and provides.
- Check how the change interacts with existing tests, configuration and persisted data.

## 3. Review passes (most important first)
1. **Correctness:** logic errors, off-by-one and boundary errors, wrong conditions, null or missing handling, type and unit mismatches, ordering and state transitions, error paths that leave inconsistent state.
2. **Contracts and compatibility:** changed signatures, schemas, outputs or defaults that break callers; missing migration or versioning.
3. **Security:** untrusted input reaching queries, shell commands, file paths, templates, deserialization or redirects; missing authentication or authorization checks; secrets in code or logs; unsafe cryptography or randomness; dependency risks. For a security-focused review, also work through [security checklist](./references/security-checklist.md).
4. **Reliability:** resource leaks, missing timeouts, unbounded retries or growth, concurrency races, non-idempotent retried operations, silent exception swallowing.
5. **Performance:** accidental quadratic work, repeated I/O or queries inside loops, loading unbounded data into memory, missing indexes or pagination where it matters.
6. **Tests:** whether the change's behavior and its edge cases are tested; tests that cannot fail; weakened assertions.
7. **Maintainability:** duplication of existing helpers, misleading names, dead code, unclear ownership. Mention only items that materially affect future correctness or clarity.

## 4. Calibrate findings
- Report concrete problems with a plausible trigger. Separate **confirmed** defects (you traced the path) from **questions** that need the author's input.
- Do not pad the review with style preferences that a formatter or linter would handle, or with generic advice.
- Never fabricate line numbers, test results or behavior. If you did not run something, do not imply you did.

## Findings format
Order findings by severity: **critical** (security hole, data loss, crash on a common path), **high** (wrong results or broken callers in realistic cases), **medium** (edge-case bug, reliability gap, missing important test), **low** (minor robustness or clarity issue). For each finding, give:
- **Location:** `path:line` or `path#symbol`.
- **Problem:** what goes wrong, and under what input or condition.
- **Evidence:** the code path or scenario that demonstrates it.
- **Fix:** the minimal correction or test to add.

End with a one-line overall assessment and anything you could not check. If nothing material is found, say so explicitly.
