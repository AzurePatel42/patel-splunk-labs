# Module 03 - Fields and Data Scenarios

## Scenario 1 - Field Existence vs Field Value

Requirement:

Find events where the status field exists, regardless of its value.

SPL:

status=*

Reasoning:

The requirement asks whether the field has a value, not for one specific status value.

Correct approach:

Use status=*.

Score:

10 / 10

---

## Scenario 2 - Specific Field Value

Requirement:

Find only events where status is 500.

SPL:

status=500

Reasoning:

The requirement specifies an exact field value.

Key distinction:

status=* checks field existence.

status=500 checks for a specific value.

Score:

10 / 10

---

## Scenario 3 - Selecting Fields

Requirement:

Keep only host, sourcetype, and status available in the current result set.

SPL:

| fields host sourcetype status

Reasoning:

fields controls which fields are retained for subsequent processing.

Score:

10 / 10

---

## Scenario 4 - Presenting Results

Requirement:

Display host, sourcetype, and status as explicit columns.

SPL:

| table host sourcetype status

Reasoning:

table explicitly formats selected fields as columns.

Key distinction:

fields controls retained fields.

table formats selected fields as columns.

Score:

10 / 10

---

## Scenario 5 - Standardizing a Field Name

Requirement:

The team wants sourcetype to appear as source_type in the current search results.

SPL:

| rename sourcetype AS source_type

Reasoning:

rename changes the field name in the current result set.

After rename:

source_type

is the active field name.

Score:

10 / 10

---

## Scenario 6 - Removing Duplicate Categories

Requirement:

Show one representative result for each unique sourcetype.

SPL:

| dedup sourcetype

Reasoning:

dedup keeps one representative row for each unique value of the specified field.

Key distinction:

dedup removes duplicate result rows.

stats count by sourcetype groups values and calculates statistics.

Score:

10 / 10

---

## Scenario 7 - Missing Status Values

Requirement:

Replace missing status values with UNKNOWN for easier analysis.

SPL:

| fillnull value="UNKNOWN" status

Reasoning:

fillnull replaces missing/null values in the current result set.

It does not modify the original indexed event.

Score:

10 / 10

---

## Scenario 8 - Investigating Available Fields

Requirement:

An engineer needs to understand which fields exist in a sample of events and what values they contain.

SPL:

| head 100
| fieldsummary

Reasoning:

fieldsummary provides a field-level profile of the current result set.

It can show information such as counts, distinct values, numeric statistics, and values.

Score:

10 / 10

---

## Scenario 9 - Pipeline State

Requirement:

Rename sourcetype and then remove duplicates using the renamed field.

SPL:

| rename sourcetype AS source_type
| dedup source_type

Reasoning:

After rename, the current result set uses source_type.

The next command must therefore reference source_type.

Incorrect:

| dedup sourcetype

Correct:

| dedup source_type

Score:

10 / 10

---

## Scenario 10 - Combined Field Pipeline

Requirement:

Keep only the required fields, rename sourcetype, replace missing status values, remove duplicate source types, and display the final result.

SPL:

index=_internal
| search status=*
| fields host sourcetype status
| rename sourcetype AS source_type
| fillnull value="UNKNOWN" status
| dedup source_type
| table host source_type status

Reasoning:

The commands operate sequentially on the current result set.

Pipeline:

search
    ->
fields
    ->
rename
    ->
fillnull
    ->
dedup
    ->
table

Key Lesson:

Always reason about what fields and values exist in the current result set before choosing the next command.

Score:

10 / 10

---

## Scenario Assessment Result

Scenarios:

10

Correct:

10

Score:

100%

Core Scenario Strength:

Pipeline state reasoning

Field existence versus field value

Command-purpose selection

Field selection and presentation

Status:

SCENARIO ASSESSMENT COMPLETE
