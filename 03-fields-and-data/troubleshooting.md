# Module 03 - Fields and Data Troubleshooting

## Troubleshooting Model

When a field-related SPL search produces an unexpected result:

1. Confirm the field exists.
2. Confirm the field values.
3. Confirm the current result set.
4. Check whether a previous command changed the field set.
5. Check whether a previous command renamed a field.
6. Check whether duplicate rows were removed.
7. Check whether missing values were replaced.
8. Check the final command and expected output format.

---

## Troubleshooting Scenario 1 - No Results for a Field

Symptom:

A search using a field returns no expected results.

Example:

status=500

Investigation:

First determine whether the status field exists and what values it actually contains.

Useful search:

| stats count by status

Key Lesson:

Do not assume a field contains a particular value. Inspect the actual values first.

---

## Troubleshooting Scenario 2 - status=* Gives More Results Than Expected

Symptom:

status=* returns many events, including events with different status values.

Investigation:

status=* tests whether the field has a value.

It does not mean:

status=200

Key Lesson:

Field existence and field value matching are different operations.

---

## Troubleshooting Scenario 3 - A Field Appears to Disappear

Symptom:

A later command cannot find a field that existed earlier.

Investigation:

Check whether a previous fields command removed it from the current result set.

Example:

| fields host sourcetype status

A later command cannot rely on fields that were not retained.

Key Lesson:

fields changes the available fields in the current result set.

---

## Troubleshooting Scenario 4 - Rename Breaks a Later Command

Symptom:

A command references sourcetype after it was renamed.

Example:

| rename sourcetype AS source_type
| dedup sourcetype

Problem:

The current result set uses source_type.

Correction:

| rename sourcetype AS source_type
| dedup source_type

Key Lesson:

Always reason from the current pipeline state.

---

## Troubleshooting Scenario 5 - Duplicate Results

Symptom:

The search contains multiple rows with the same category.

Investigation:

Determine whether the requirement is to remove duplicates or calculate statistics.

For one representative row per value:

| dedup sourcetype

For counts by value:

| stats count by sourcetype

Key Lesson:

dedup and stats solve different problems.

---

## Troubleshooting Scenario 6 - Blank Field Values

Symptom:

Some rows contain blank status values.

Investigation:

If the requirement is to make missing values explicit:

| fillnull value="UNKNOWN" status

Key Lesson:

fillnull changes the current search result representation.

It does not modify the original indexed event.

---

## Troubleshooting Scenario 7 - Wrong Output Format

Symptom:

The required fields are available, but the result is not displayed in the desired column format.

Investigation:

Use table when the requirement is to explicitly format selected fields as columns.

Example:

| table host sourcetype status

Key Lesson:

fields and table have different purposes.

---

## Troubleshooting Scenario 8 - Unexpected field statistics

Symptom:

An engineer expects fieldsummary to describe the entire index but receives statistics based on a limited sample.

Investigation:

Check the commands before fieldsummary.

Example:

| head 100
| fieldsummary

fieldsummary operates on the current result set.

Key Lesson:

Always determine what data reaches the command being analyzed.

---

## Troubleshooting Scenario 9 - Field Values Look Unexpected

Symptom:

A field expected to contain numeric HTTP status codes also contains values such as Ready or success.

Investigation:

Use:

| stats count by status
| sort - count

This reveals the actual values in the current result set.

Key Lesson:

Do not assume a field's values based only on its name.

---

## Troubleshooting Scenario 10 - Complex Pipeline Produces Unexpected Rows

Symptom:

A combined search produces fewer rows than expected.

Example:

| fields host sourcetype status
| rename sourcetype AS source_type
| fillnull value="UNKNOWN" status
| dedup source_type
| table host source_type status

Investigation:

Trace the pipeline from left to right.

Pipeline:

fields
    ->
rename
    ->
fillnull
    ->
dedup
    ->
table

The dedup command can reduce the number of rows because it keeps one representative row per unique source_type.

Key Lesson:

When troubleshooting SPL, identify which command changed the result set before investigating the final output.

---

## Troubleshooting Principles

Principle 1:

Inspect before assuming.

Principle 2:

Reason about the current result set.

Principle 3:

Know whether a command filters, retains, renames, removes duplicates, replaces values, or analyzes fields.

Principle 4:

Trace the pipeline from left to right.

Principle 5:

Use evidence from actual field values.

---

## Troubleshooting Assessment Result

Scenarios:

10

Documented:

10

Core Troubleshooting Strength:

Pipeline-state reasoning

Field-value inspection

Command-purpose identification

Result-set analysis

Status:

TROUBLESHOOTING DOCUMENTATION COMPLETE
