# Module 03 - Fields and Data Labs

## Lab 1 - Table Selected Fields

SPL:

index=_internal
| head 10
| table _time host source sourcetype

Result:

10 events returned.

Observed fields:

_time
host
source
sourcetype

Key Lesson:

table explicitly formats selected fields as columns.

Fields not included in the table are not deleted from the indexed events. They are simply not included in the displayed result.

Score:

10 / 10

---

## Lab 2 - Field Existence

SPL:

index=_internal
| search status=*
| stats count

Result:

13,157 events matched status=*.

Key Lesson:

status=* checks whether the status field has a value. It does not enumerate the possible status values.

Score:

10 / 10

---

## Lab 3 - Field Values

SPL:

index=_internal
| search status=*
| stats count by status
| sort - count

Result:

15 status values were observed.

Examples included:

200
404
201
409
Starting
204
400
403
Ready
304
303
503
401
500
success

Key Lesson:

status=* checks field existence, while stats count by status enumerates actual field values and counts them.

The status field can contain both numeric-looking and nonnumeric values.

Score:

10 / 10

---

## Lab 4 - fields

SPL:

index=_internal
| search status=*
| fields host sourcetype status
| head 10

Result:

10 events returned.

Key Lesson:

fields controls which fields are retained in the current result set.

Score:

10 / 10

---

## Lab 5 - table

SPL:

index=_internal
| search status=*
| table host sourcetype status
| head 10

Result:

13,430 events matched status=* and 10 rows were displayed.

Displayed columns:

host
sourcetype
status

Key Lesson:

table explicitly formats selected fields as columns.

Score:

10 / 10

---

## Lab 6 - rename

SPL:

index=_internal
| search status=*
| table host sourcetype status
| rename sourcetype AS source_type
| head 10

Result:

10 rows displayed.

Key Lesson:

rename changes the field name in the current result set.

sourcetype -> source_type

The field values remained unchanged.

Score:

10 / 10

---

## Lab 7 - dedup

SPL:

index=_internal
| table host sourcetype status
| dedup sourcetype
| head 10

Result:

10 unique sourcetype values were displayed.

Key Lesson:

dedup keeps one representative row for each unique value of the specified field.

dedup sourcetype removes duplicate result rows based on sourcetype.

This differs from stats count by sourcetype, which groups values and calculates statistics.

Score:

10 / 10

---

## Lab 8 - fillnull

SPL:

index=_internal
| table host sourcetype status
| fillnull value="UNKNOWN" status
| head 10

Result:

10 rows displayed.

Previously blank status values were displayed as UNKNOWN.

Key Lesson:

fillnull replaces missing/null field values in the current search result set.

It does not modify the original indexed events.

Score:

10 / 10

---

## Lab 9 - fieldsummary

SPL:

index=_internal
| head 100
| fieldsummary

Result:

100 events were analyzed.

fieldsummary produced 180 field-summary rows.

The output included field-level information such as:

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

Key Lesson:

fieldsummary provides a field-level profile of the current result set.

It helps identify what fields exist and what kinds of values they contain.

Score:

10 / 10

---

## Lab 10 - Combined Pipeline

SPL:

index=_internal
| search status=*
| fields host sourcetype status
| rename sourcetype AS source_type
| fillnull value="UNKNOWN" status
| dedup source_type
| table host source_type status
| head 10

Result:

5 unique source_type values were displayed.

Observed values included:

splunkd_ui_access
splunkd_access
splunk_web_access
scheduler
node:sidecar:postgres:stdout

Observed status values included:

200
success
Ready

Key Lesson:

Each command operates on the result set produced by the preceding command.

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
    ->
head

Score:

10 / 10

---

## Final Hands-On Score

Labs Completed:

10 / 10

Overall Hands-On Score:

100%

Status:

HANDS-ON LABS COMPLETE
