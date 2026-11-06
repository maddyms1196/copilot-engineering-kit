# Statistical and Modelling Checks

## Descriptive work
- Report the sample size with every statistic. Use medians and quantiles alongside means for skewed data.
- Plot or tabulate distributions before summarizing them; check for heavy tails, multimodality and spikes at sentinel values (0, -1, 999, empty strings).
- When comparing groups, check that their composition is comparable. Simpson's paradox appears when segment mix differs.

## Inference
- State the hypothesis, test and significance threshold before looking at results.
- Check the test's assumptions (independence, distribution shape, equal variance) or use a robust or nonparametric alternative, or a bootstrap.
- Report effect sizes with confidence intervals, not only p-values. Distinguish statistical significance from practical importance.
- Correct for multiple comparisons when testing many metrics or segments, or label the findings exploratory.
- Avoid peeking: repeated testing of an experiment while it runs inflates false positives unless a sequential method is used.

## Experiments (A/B tests)
- Verify randomization: check sample-ratio mismatch and pre-period balance of key covariates.
- Analyze at the unit of randomization; cluster or aggregate when units are not independent.
- Watch for novelty effects, interference between units, and changes in the metric's definition.

## Predictive models
- Establish a naive baseline (mean, last value, majority class) and compare against it.
- Check for leakage: features computed using the target or future information, duplicates shared between train and test, preprocessing fitted on all data, or random splits on time-ordered or grouped data.
- Use a validation design that matches deployment: time-based splits for forecasting, group splits for repeated entities.
- Evaluate against the original observed targets, in units that matter in use. Reversible target transformations may be applied consistently and inverted for metrics; capping, clipping or filtering evaluation labels can hide exactly the errors you are looking for. Document any exclusions.
- Evaluate beyond one score: calibration, residuals against predictions and key features, errors by segment, and behavior on rare classes or extremes.
- For systematic bias (for example consistent under-prediction), inspect target transformations and their inverses, regularization strength, distribution shift between training and scoring data, clipped or capped targets, and missing features for the affected segment.
- Fix random seeds and record library versions so that results can be reproduced.

## Time series
- Check for missing timestamps, duplicates, time-zone shifts, daylight-saving artifacts and changes in the collection process.
- Account for trend and seasonality before calling a point anomalous; compare against the same period in earlier cycles.

## Anomalies
- Define "normal" from a relevant reference (peers, history, expected range) before labelling something anomalous.
- Rule out data-pipeline explanations (late loads, duplicates, schema changes, unit changes) before business explanations.
