---
name: performance-optimization
description: Measure and improve runtime, memory, throughput, latency, query time or scalability without changing results. Use when code, a test suite, a build, a query, a page or a data pipeline is slow, uses too much memory, times out, or needs to handle larger inputs, or when asked to profile, benchmark, speed up or optimize something.
argument-hint: "[slow code, query, or workload]"
---

# Measured Performance Optimization

No optimization claim without a measurement. Correctness comes first.

## 1. Define the target
- Choose the metric (wall time, CPU, peak memory, p50 or p95 latency, throughput, query cost), the workload (realistic input size and shape), and the goal or budget.
- Note the constraints: identical outputs or an explicit numeric tolerance, API compatibility, memory limits and allowable dependencies.

## 2. Baseline
- Create or find a repeatable benchmark on representative input, using tools that already exist in the environment. See [profiling tools](./references/profiling-tools.md) for common options by ecosystem.
- Warm up, then repeat several runs and report the median and spread. Record the command, input size and environment notes.
- Capture a reference output for the equivalence checks later.

## 3. Find the bottleneck
- Profile before changing code; optimize the measured hot path, not the suspected one.
- Common causes: algorithmic complexity (nested loops, repeated linear lookups, sorting inside loops), repeated I/O or N+1 queries, unnecessary serialization or copying, missing indexes or batching, per-element work that the language can vectorize, lock contention, and cache misses or recomputation.
- Estimate the possible gain (the bottleneck's share of total time) before investing effort.

## 4. Change one thing at a time
- Prefer the lowest-risk, highest-gain change: a better algorithm or data structure, batching, caching with a correct invalidation rule, moving work out of loops, or pushing filters and aggregation into the database.
- Preserve observable behavior, including ordering and tie-breaking, error behavior, laziness and streaming, floating-point accumulation order where it matters, and side effects.
- Require evidence before adding dependencies or concurrency. Use authorization already given, and ask before consequential actions beyond it. Avoid unsafe tricks.

## 5. Verify
- Check equivalence: compare outputs with the reference on the benchmark input plus edge cases (empty input, ties, duplicates, extremes). Run the existing tests.
- Re-measure under the same conditions. Report before and after values with spread, and check that small inputs did not regress.
- If measurement is impossible in this environment, label the change as an unmeasured hypothesis and give the exact benchmark command to run.

## Report
Baseline (command, input, numbers); the measured bottleneck; the change made; after numbers and speed-up; equivalence and test results; trade-offs (memory, complexity) and caveats.
