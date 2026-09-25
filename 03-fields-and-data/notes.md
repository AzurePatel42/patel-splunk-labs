# Module 03 - Fields and Data

## 1. What Is a Field?

A field is a named piece of information extracted from an event.

Examples:

host
source
sourcetype
status

A field contains a field value.

Example:

Field:

status

Field value:

200

---

## 2. Field vs Field Value

Field:

status

Field value:

200

The field is the name or attribute.

The field value is the data stored in that field for a particular event.

---

## 3. Why Fields Matter

Fields allow SPL searches to filter, organize, transform, and analyze event data.

Examples:

search status=200

stats count by sourcetype

table host sourcetype status

Fields are the primary way to reason about structured information inside Splunk events.

---

## 4. Field Existence vs Field Value

These are different questions.

Field existence:

status=*

This asks whether the status field has a value.

Specific field value:

status=200

This asks for events where the status field matches 200.

To enumerate actual values:

stats count by status

---

## 5. Types of Fields

Fields can come from different sources.

Automatically extracted fields:

Fields extracted from event data by Splunk.

Metadata fields:

Examples include:

_time
host
source
sourcetype

SPL-created fields:

Commands such as eval can create new fields.

Example:

eval volume=case(count>200,"HIGH",true(),"LOW")

---

## 6. Pipeline State

SPL commands operate on the current result set produced by the previous command.

Example:

| rename sourcetype AS source_type

After this command, the current result set uses source_type.

A later command must therefore reference:

source_type

not:

sourcetype

Mental model:

Current command
    ->
changes or filters current result set
    ->
next command operates on that result set

---

## 7. fields

The fields command controls which fields are retained in the current result set.

Example:

fields host sourcetype status

This keeps the specified fields available for subsequent processing.

---

## 8. table

The table command explicitly formats selected fields as columns.

Example:

table host sourcetype status

The purpose is to produce a clear tabular result.

Key distinction:

fields controls fields retained.

table explicitly formats selected fields as columns.

---

## 9. rename

The rename command changes a field name in the current result set.

Example:

rename sourcetype AS source_type

After the command:

source_type

exists instead of:

sourcetype

The field values remain the same unless another command changes them.

---

## 10. dedup

The dedup command removes duplicate result rows based on one or more specified fields.

Example:

dedup sourcetype

This keeps one representative row for each unique sourcetype value.

Key distinction:

dedup removes duplicate rows.

stats count by sourcetype groups values and calculates statistics.

---

## 11. fillnull

The fillnull command replaces missing or null field values in the current result set.

Example:

fillnull value="UNKNOWN" status

A missing status value can then appear as:

UNKNOWN

This changes the current search result representation.

It does not modify the original indexed event.

---

## 12. fieldsummary

The fieldsummary command provides a field-level profile of the current result set.

It can provide information such as:

field
count
distinct_count
is_exact
max
mean
min
numeric_count
stdev
values

It helps answer:

What fields exist?

How often do they occur?

What values do they contain?

What kinds of values are present?

---

## 13. Core Module 03 Mental Model

Raw Events
    ->
Fields
    ->
Field Values
    ->
Filtering
    ->
Field Selection
    ->
Field Renaming
    ->
Duplicate Handling
    ->
Missing-Value Handling
    ->
Field Analysis

---

## 14. Golden Question

When working with fields in SPL, ask:

What fields exist in my current result set, and what do their values represent?

Then determine:

Which fields do I need?

Which values do I need?

What should the result set look like after this command?

What will the next command receive?

---

## 15. Core Command Summary

fields:

Controls which fields are retained.

table:

Explicitly formats selected fields as columns.

rename:

Changes a field name.

dedup:

Removes duplicate rows based on specified fields.

fillnull:

Replaces missing/null field values.

fieldsummary:

Provides field-level statistics and value information.

---

## Module Status

Theory:

COMPLETE

Brainstorming:

COMPLETE

Hands-On Labs:

COMPLETE

Overall Hands-On Score:

100%

Next:

Questions and Formal Assessment
