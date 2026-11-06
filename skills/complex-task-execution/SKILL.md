---
name: complex-task-execution
description: Plan, decompose, track and finish large or multi-step engineering tasks with evidence-based completion. Use for requests spanning several files or components, features with multiple parts, migrations, sweeping changes ("everywhere", "across the codebase", "all of"), long investigations, or any task likely to need more than about three distinct steps or to outlast one context window.
argument-hint: "[goal or acceptance criteria]"
---

# Complex Task Execution

## 1. Define done before starting
- Restate the goal as a short list of acceptance criteria that can be observed: behavior, outputs, tests, measurements.
- List constraints: what must not change (public interfaces, data formats, behavior outside scope), compatibility targets, performance or security limits.
- Resolve ambiguity by discovery first (code, tests, docs, history). Ask the user only for decisions that would change the outcome and cannot be found or defaulted; record every assumption.

## 2. Decompose
- Break the work into steps that can each be verified on their own. Order them by dependency, and do the riskiest or least-understood step first (reproduce the bug, try a spike, confirm an API exists).
- Prefer vertical slices that leave the code working after each step over one large change at the end.
- For a sweeping change, first enumerate every affected site with a search and record the count, so completion can be checked against it.

## 3. Track progress
- Use the todo tool when it is available; otherwise keep a short numbered checklist in your messages.
- Keep exactly one item in progress. Mark an item done only after its check passes, and note the evidence (command and result, or file and line).
- When you discover new work, add it as a new item instead of silently widening the current one. Flag out-of-scope discoveries for the user rather than doing them.
- Keep the list current enough that the work could resume from it alone if earlier conversation context is summarized or lost. Record key findings, decisions and failed attempts (what was tried and why it failed) so they are not rediscovered or repeated.
- Re-read the goal and remaining items before major decisions. Write a plan or notes file only when the user asks, the project already keeps one, or the work will clearly span sessions; keep it out of deliverables.

## 4. Execute in checkpoints
- After each step, run its check (targeted test, build, type check, or reproduction). Do not build on a failing step unless that is intended and noted.
- If the same approach fails twice, stop and re-examine: reread the error, recheck assumptions, consider a different hypothesis, or narrow the problem. Do not loop on small variations.
- If blocked by a permission, missing tool, missing data or a decision only the user can make, finish everything else that does not depend on it, then report the exact blocker and what is needed.

## 5. Delegate selectively
- Consider delegating broad read-only research when the context saving justifies it. Prefer a suitable built-in specialist, or **Explorer**, with a self-contained brief including relevant project rules and a requested output format. Continue directly if delegation is unavailable.
- Run independent investigations as separate subagent tasks when the harness allows; keep dependent steps in the main agent.
- After substantial or risky implementation, consider an independent review when a suitable subagent is available. Supply the diff, changed-file list and relevant project rules to a reviewer that cannot obtain them. Verify its findings before acting; otherwise perform a focused self-review.
- Do not delegate edits that need the main conversation's context, or tasks small enough to do directly.

## 6. Completion audit
Before reporting completion:
1. Walk through each acceptance criterion and cite the evidence that it is met.
2. Run the broadest relevant test, build or lint suite that is permitted and practical. Separate new failures from pre-existing ones.
3. For sweeping changes, re-run the enumeration search and confirm nothing was missed.
4. Review the full diff for accidental edits, debugging leftovers, secrets and unrelated formatting.
5. List anything unverified, deferred or assumed.

## Report
Outcome; criteria met, each with its evidence; files changed; checks run and their results; assumptions; remaining risks, blockers or follow-ups.
