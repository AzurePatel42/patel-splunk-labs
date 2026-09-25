# Module 03 - Fields and Data Questions

## Formal Assessment

### Q1

What is the difference between a field and a field value?

A:

The field is the name or attribute, while the field value is the data stored in that field.

Answer:

A

Score:

10 / 10

---

### Q2

What does the following search primarily test?

status=*

A:

Whether the status field has a value

B:

Whether status equals the literal value *

C:

Whether status is exactly 200

D:

Whether every event contains the same status value

Answer:

A

Score:

10 / 10

---

### Q3

Which command is used to control which fields are retained in the current result set?

A:

table

B:

rename

C:

fields

D:

dedup

Answer:

C

Score:

10 / 10

---

### Q4

What is the primary purpose of the table command?

A:

Perform statistical aggregation

B:

Explicitly format selected fields as columns

C:

Remove duplicate values

D:

Replace missing values

Answer:

B

Score:

10 / 10

---

### Q5

What does the following command do?

rename sourcetype AS source_type

A:

Creates a second field while keeping sourcetype unchanged

B:

Changes the field name from sourcetype to source_type

C:

Deletes the sourcetype values

D:

Groups events by source_type

Answer:

B

Score:

10 / 10

---

### Q6

What does dedup sourcetype do?

A:

Counts the number of events for each sourcetype

B:

Keeps one representative row for each unique sourcetype value

C:

Renames sourcetype

D:

Replaces missing sourcetype values

Answer:

B

Score:

10 / 10

---

### Q7

Which SPL command can create a new field during the search pipeline?

A:

head

B:

eval

C:

dedup

D:

table

Answer:

B

Score:

10 / 10

---

### Q8

What does the following command do?

fillnull value="UNKNOWN" status

A:

Replaces missing/null status values with UNKNOWN

B:

Deletes events where status is missing

C:

Changes all status values to UNKNOWN

D:

Creates a new status field in the indexed data

Answer:

A

Score:

10 / 10

---

### Q9

What does fieldsummary provide?

A:

A list of all indexes

B:

A field-level profile of the current result set

C:

A list of users and roles

D:

A list of dashboards

Answer:

C

Score:

10 / 10

---

### Q10

Consider:

index=_internal
| stats count by sourcetype
| where count > 50

What does where count > 50 filter?

A:

Raw events before stats runs

B:

The original indexed events

C:

The result rows produced by stats

D:

The sourcetype field definition

Answer:

C

Score:

10 / 10

---

## Formal Assessment Result

Questions:

10

Correct:

9

Score:

90%

Weak Areas:

Field existence versus specific field value

Command purpose distinctions

Pipeline state reasoning

Status:

FORMAL ASSESSMENT COMPLETE
