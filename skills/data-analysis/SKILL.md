---
name: data-analysis
description: Perform reproducible, evidence-based data analysis with Python, SQL, notebooks or spreadsheets, including exploratory analysis, data-quality checks, statistics, experiment or A/B results, anomaly investigation and model evaluation. Use for questions about datasets, CSV, Parquet or Excel files, database tables, queries, metrics, distributions, correlations, trends, forecasts, or why a number or model looks wrong.
argument-hint: "[dataset or question]"
---

# Evidence-Based Data Analysis

Work from the actual data and environment. Never invent schemas, values, metrics or results.

## 1. Frame the question
- State the question, the unit of observation (one row is what?), the population and time window, the metric definitions, and the comparison or baseline.
- Decide what result would answer the question before computing anything.

## 2. Discover the environment (no installs)
- Find what is already available: the language runtime, existing project libraries (for example pandas, polars, numpy, scipy, statsmodels, scikit-learn, DuckDB, or the standard-library `csv`, `sqlite3` and `statistics` modules), notebooks, database clients and existing analysis code.
- Use already authorized data sources and tools; ask before new external access or package installation beyond that scope. Local scratch databases for analysis of supplied files are routine. If tooling is missing, use what exists or explain the gap.
- Use read-only connections for existing databases where supported. A `SELECT` can invoke side-effecting functions; inspect the query and access mode. Writes to existing or shared data require authorization for that operation and target.

## 3. Inspect before analyzing
- Look at the schema and a small sample first: column names and types, row counts, keys and their uniqueness, date ranges and time zones, units and currencies, category spellings.
- Profile quality: missing values, duplicates, impossible or negative values, outliers, inconsistent labels, mixed units, and gaps in time series.
- On large tables, use `LIMIT`, sampling, partition filters or aggregates first. Estimate cost before full scans on metered warehouses.
- For SQL-specific checks, see [SQL checks](./references/sql-checks.md).

## 4. Analyze
- Start with descriptive statistics and a simple baseline before complex methods.
- For joins, check key uniqueness, unmatched keys, row counts and totals before and after. Row growth can reveal fan-out; row loss can reveal filtering or unmatched keys. Compare against the intended relationship rather than assuming every change is a defect.
- Respect time order: no look-ahead, no future data in features, and splits by time when forecasting.
- Compare segments and look for confounding before attributing causes. A correlation is not an effect.
- For statistical testing, modelling and experiments, see [statistical checks](./references/statistical-checks.md).

## 5. Validate
- Cross-check key numbers with an independent method: a different query path, a hand-computed small case, or reconciliation against a known total.
- Investigate surprising results before reporting them; most surprises are data or join errors.
- Keep the work reproducible: a script, query or notebook cell that regenerates every reported number from the source.

## 6. Privacy and safety
- Report aggregates. Do not print, copy or export raw rows containing personal or sensitive data. Mask identifiers in examples.
- Write cleaned or derived data to a new file and say where. Do not overwrite source data unless asked; if you do, keep an exact copy first.
- Do not move data to external services, and do not save outputs outside the workspace without approval.

## Report
The question; data scope (source, rows, period, filters); quality issues found and how they were handled; methods actually run; findings with numbers, units, sample sizes and uncertainty; caveats and alternative explanations; how to reproduce. Mark anything not executed as a proposal, not a result.
