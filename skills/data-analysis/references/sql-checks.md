# SQL Checks

## Before trusting a query
- **Grain:** what does one output row represent? Make it explicit with `GROUP BY` or a uniqueness check.
- **Key uniqueness:** `SELECT key, COUNT(*) FROM t GROUP BY key HAVING COUNT(*) > 1` on each side of a join.
- **Row-count reconciliation:** compare counts and left-side key multiplicity before and after joins and filters. Repeated left-side rows can inflate their measures; compare against the intended cardinality before calling fan-out a defect.
- **Join type:** an `INNER JOIN` drops unmatched rows. A null-rejecting `WHERE` predicate on the right side of a `LEFT JOIN` (for example `right.qty > 0`) removes unmatched rows; `right.key IS NULL` instead selects unmatched rows. Count unmatched keys explicitly.
- **NULL semantics:** `NULL = NULL` is not true; `NOT IN` with a NULL in its right-hand result can evaluate to UNKNOWN and exclude otherwise unmatched rows; `COUNT(col)` skips NULLs while `COUNT(*)` does not; `AVG` ignores NULLs. `col <> 'x'` (or `!=`) is not true when `col` is NULL, so those rows are silently dropped; write `col IS NULL OR col <> 'x'` when NULL rows belong in the result.
- **Aggregation:** do not sum pre-aggregated averages or ratios; aggregate numerators and denominators separately. Watch for double counting when aggregating after a fan-out join.
- **Distinct as a patch:** `SELECT DISTINCT` that "fixes" duplicates usually hides a join or grain error. Find the cause instead.
- **Time:** confirm time zones, inclusive versus exclusive date bounds (`<` the next day rather than `<= 23:59:59`), and late-arriving data.
- **Types:** integer division, implicit casts, string-to-date parsing, and currency or unit columns.
- **Window functions:** verify `PARTITION BY` and `ORDER BY`, add a deterministic tie-breaker, and check frame clauses (`ROWS` versus `RANGE`).

## Cost and safety
- Explore with partition filters, plan estimates and small results. `LIMIT` bounds returned rows but may still scan a large table; check estimated cost before running on a metered warehouse.
- Use a read-only connection for existing databases where supported; a `SELECT` can call side-effecting functions. Writes, DDL and temporary objects on shared databases require authorization for that target. A new local scratch database for supplied files is acceptable within an analysis task.
- Never paste credentials into queries or scripts; use the project's existing connection configuration.

## Reporting
Include the final query (or a reference to the file containing it), the row counts at key stages, and the reconciliation used to validate totals.
