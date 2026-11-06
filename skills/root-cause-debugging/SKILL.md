---
name: root-cause-debugging
description: Diagnose and fix defects by reproducing them, testing competing hypotheses and tracing to the earliest cause. Use for errors, exceptions, stack traces, failing or flaky tests, crashes, hangs, wrong outputs, regressions ("it used to work"), environment and configuration problems, and data or model results that look wrong.
argument-hint: "[error, failing test, or symptom]"
---

# Root-Cause Debugging

Solve the reported behavior, not only the visible message. A diagnosis-only request gets evidence and a proposed fix. A request to fix (including "can you fix…" or a bare symptom in an active coding task) gets implementation and verification, subject to explicit limits and granted tools.

## 1. Pin down the failure
- Expected versus actual behavior, the exact error text, first point of failure, affected inputs or environments, and when it started if known.
- Gather the evidence that already exists: stack traces, logs, failing test output, recent changes, configuration and data samples. Read the stack trace from the innermost frame that is in project code.

## 2. Reproduce
- Run the smallest command that shows the failure (a single test, script or request) when execution is permitted. Record the command and its output.
- If you cannot reproduce it, say why (no permission, missing environment, external dependency, or nondeterminism) and continue with static analysis, labelled as such.
- For intermittent failures, run several times, and check ordering dependence, shared state, time, randomness without a fixed seed, concurrency, caching and network.

## 3. Hypothesize and discriminate
- When the cause is not obvious, list two or three plausible causes, ranked by evidence.
- For each, choose the cheapest check that would rule it in or out: a targeted log or print, an assertion, a debugger breakpoint, a narrowed input, a toggled flag, or a comparison against a known-good version.
- Change one variable at a time. Update the ranking after each result. Correlation is not cause; confirm with an intervention.
- Localize by bisection when the space is large: halve the input, halve the pipeline stages, or (if version control is available read-only) compare against a known-good revision.

## 4. Find the root
- Trace data and control flow backward from the symptom to the earliest point where reality diverges from intent. Distinguish the root cause from downstream symptoms and from contributing factors.
- Look for the same defect pattern in sibling code (other callers, copy-pasted variants, the same off-by-one or null-handling style). Report each instance, and fix them if they fall within scope.

## 5. Fix (when requested)
- Change code only once you can state the cause and the evidence for it. Trial-and-error edits are experiments: revert them if they do not confirm the hypothesis.
- If two fix attempts fail, stop patching. Return to investigation, re-examine the assumptions and consider whether the design itself is the problem.
- Make the smallest change that corrects the cause and follows local conventions. Do not mask the problem with broad exception handling, retries, sleeps, default values or weakened assertions.
- Add or update a regression test that fails before the fix and passes after it, when the project has tests.
- Remove temporary instrumentation.

## 6. Verify
- Re-run the original reproduction, the new regression test and neighbouring tests. Then run the broader suite in proportion to risk.
- Confirm no new failures. Report pre-existing failures separately.

## Data and model bugs
Check row counts at each stage, join cardinality, null handling, type coercion, time zones, units, sorting and stability assumptions, train/serve or train/test skew, leakage, and random seeds. Compare a small hand-computed case to the program's output.

## If the cause stays unknown
Report what has been ruled out (with evidence), the remaining hypotheses, and the single most informative next check.

## Report
Symptom and reproduction (command and result); root cause with evidence (`path:line`); fix applied, or proposed if diagnosis only; verification run and results; related risks or sibling instances.
