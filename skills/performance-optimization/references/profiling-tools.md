# Profiling and Benchmarking Options

Use only what is already available in the project or the standard toolchain. Do not install profilers without approval. Confirm the availability of a tool before relying on it.

| Ecosystem | Timing and benchmarks | CPU profiling | Memory |
| --- | --- | --- | --- |
| Python | `timeit`, `time.perf_counter`, `pytest-benchmark` if already a dependency | `python -m cProfile -s cumulative script.py`, `pstats` | `tracemalloc` (standard library) |
| JavaScript / TypeScript (Node) | `performance.now()`, `console.time`, existing benchmark scripts | `node --cpu-prof`, `node --prof` | `node --heap-prof`, `process.memoryUsage()` |
| Browser front end | Performance panel in developer tools, Lighthouse if available | Performance panel flame chart | Memory panel heap snapshots |
| Java / Kotlin | JMH if already present; repeated timed runs after warm-up | Java Flight Recorder (`-XX:StartFlightRecording`, or `jcmd <pid> JFR.start`) | JFR allocation events, heap dumps |
| Go | `go test -bench . -benchmem` | `go test -cpuprofile`, `go tool pprof` | `-memprofile`, `pprof` |
| Rust | `cargo bench` if configured | `perf` or platform profilers if present | Allocation counters if present |
| .NET | BenchmarkDotNet if already present; `Stopwatch` | `dotnet-trace` if installed | `dotnet-counters`, `dotnet-gcdump` if installed |
| SQL | `EXPLAIN` / `EXPLAIN ANALYZE` (engine-specific; `ANALYZE` executes the query), query history and statistics views | Query plan inspection: scans, join order, estimated versus actual rows | Spill-to-disk and memory grants in plan output |
| Shell / CLI | `time` and repeated runs | Platform profilers if available | `/usr/bin/time -v` (Linux) or `-l` (macOS) |

## Measurement hygiene
- Warm up caches and JIT compilers before timing; report the median of several runs.
- Keep input, hardware and background load comparable between runs.
- Separate one-time costs (startup, compilation, connection) from steady-state costs.
- For databases, note cache state, and avoid running `EXPLAIN ANALYZE` on statements that modify data.
