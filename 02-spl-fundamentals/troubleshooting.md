# Module 02 - Troubleshooting

## Troubleshooting Assessment

Questions:

10

Final Score:

96%

---

## Troubleshooting Results

T1:

10 / 10

T2:

6 / 10

T3:

10 / 10

T4:

10 / 10

T5:

10 / 10

T6:

10 / 10

T7:

10 / 10

T8:

10 / 10

T9:

10 / 10

T10:

10 / 10

---

## Troubleshooting Focus

The troubleshooting exercises focused on diagnosing SPL pipelines by tracking the current result set and identifying when fields are created, filtered, transformed, or replaced.

Key concepts reinforced:

- `stats` must create a field before later commands can evaluate that field.
- `where` filters the current result rows.
- `stats count` counts current result rows.
- `stats sum(count)` sums existing `count` values.
- A later transforming `stats` command can create a new result set.
- Fields not included in a later transforming command may disappear from the resulting output.
- `eval` can classify existing statistical fields when those fields are available.
- Aggregation thresholds must be evaluated against the correct current result set.
- The final result count can differ substantially from the original raw event count after filtering and transformation.

## Primary Troubleshooting Model

When troubleshooting a multi-command SPL pipeline, verify:

1. What result set exists at the current stage?
2. Which fields currently exist?
3. Was the required field created before it was referenced?
4. Does `where` filter raw events or current result rows?
5. Does the next `stats` count rows or aggregate an existing field?
6. Which fields are preserved by a transforming command?

---

## Status

COMPLETE

Final Score:

96%

Next:

Weak-Area Retest
