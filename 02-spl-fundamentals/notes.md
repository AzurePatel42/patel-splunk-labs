# Splunk Core - 02 SPL Fundamentals

## Module Status

Theory:

COMPLETE

---

# SPL Fundamentals

## 1. SPL Pipeline

Splunk SPL commands are connected using the pipe character:

|

The pipe passes the results from one command to the next command.

Example:

index=_internal
| search error
| stats count by host

Execution flow:

Retrieve events
    ->
Filter events
    ->
Aggregate results
    ->
Display results

The output of one command becomes the input to the next command.

---

## 2. search Command

The search command filters events.

Example:

index=_internal
| search error

Meaning:

Retrieve events from the _internal index
    ->
Keep events containing error
    ->
Return the remaining events

Core concept:

search = FILTER EVENTS

---

## 3. where Command

The where command evaluates conditions against fields in the current result set.

Example:

index=_internal
| where status="error"

Core concept:

where = FILTER USING FIELD CONDITIONS

Important distinction:

search
    ->
Search/refine events

where
    ->
Evaluate a condition against fields

---

## 4. stats Command

The stats command transforms the current event stream into statistical results.

Example:

index=_internal
| stats count by host

The result is a table containing:

host
count

stats does not return the original raw events.

Core concept:

stats = TRANSFORM / AGGREGATE

---

## 5. sort Command

The sort command controls the order of the results.

Example:

index=_internal
| stats count by host
| sort - count

The minus sign means descending order.

Execution:

Count events by host
    ->
Sort hosts by count
    ->
Highest count first

Core concept:

sort - count = DESCENDING COUNT

---

## 6. eval Command

The eval command creates or calculates fields.

Example:

index=_internal
| eval severity=if(status="error", "HIGH", "NORMAL")

This creates a new field:

severity

Possible values:

HIGH
NORMAL

Core concept:

eval = CREATE / CALCULATE FIELD

---

## 7. eval Followed by stats

Commands execute from left to right.

Example:

index=_internal
| eval severity=if(status="error", "HIGH", "NORMAL")
| stats count by severity

Execution:

Retrieve events
    ->
Create severity field
    ->
Count events by severity
    ->
Return statistical table

Core reasoning:

eval creates/calculates
    ->
stats aggregates

---

## 8. Modifying an Existing Field

eval can also modify an existing field.

Example:

index=_internal
| eval status=if(status="error", "HIGH", status)

The existing status field is changed for matching events.

Core concept:

eval can CREATE a field or MODIFY an existing field.

---

## 9. case() Function

case() evaluates multiple conditions.

Example:

index=_internal
| eval severity=case(
    status="error", "HIGH",
    status="warning", "MEDIUM",
    true(), "LOW"
)

Evaluation occurs from top to bottom.

First true condition wins.

true() provides the fallback condition.

Logic:

error
    ->
HIGH

warning
    ->
MEDIUM

Anything else
    ->
LOW

Core concept:

case() = MULTIPLE CONDITIONAL BRANCHES

---

## 10. Combined SPL Pipeline

Example:

index=_internal
| eval severity=case(
    status="error", "HIGH",
    status="warning", "MEDIUM",
    true(), "LOW"
)
| stats count by severity
| sort - count

Execution model:

Retrieve
    ->
Create / calculate fields
    ->
Aggregate
    ->
Sort
    ->
Interpret

---

# Core SPL Reasoning Model

Requirement
    ->
Retrieve Data
    ->
Filter
    ->
Create / Calculate Fields
    ->
Aggregate / Transform
    ->
Sort / Present
    ->
Interpret

---

# Key Command Model

search
    ->
FILTER EVENTS

where
    ->
FILTER USING FIELD CONDITIONS

eval
    ->
CREATE / CALCULATE / MODIFY FIELDS

stats
    ->
AGGREGATE / TRANSFORM

sort
    ->
ORDER RESULTS

case()
    ->
MULTIPLE CONDITIONAL BRANCHES

---

# Module 02 Theory Checkpoint

Questions:

10 / 10

Score:

90%

Strong Areas:

- SPL pipeline reasoning
- search filtering
- where filtering
- stats aggregation
- sort ordering
- eval field creation
- eval field modification
- eval followed by stats
- case() logic
- combined pipeline reasoning

Weak Area:

- Distinguishing where filtering from stats transformation

---

# Current Status

02 - SPL Fundamentals

Theory:

COMPLETE

Next:

Brainstorming
    ->
Hands-On SPL Labs
    ->
Formal Questions
    ->
Scenarios
    ->
Troubleshooting
    ->
Weak-Area Identification
    ->
Retest

---

# SPL Fundamentals - Brainstorming

## Brainstorming Status

COMPLETE

Questions:

10 / 10

---

## Question 1 - SPL Pipeline Execution

Example:

index=_internal
| search error
| stats count by host
| sort - count

Execution:

Retrieve events from _internal
    ->
Keep matching events containing the search term error
    ->
Count the remaining events by host
    ->
Sort the result by count in descending order

Important distinction:

search error

does not necessarily mean searching for a field named error.

It can be a search term used to refine the event set.

---

## Question 2 - search vs where

Example:

index=_internal
| where status="error"
| stats count by host

where evaluates the value of a specific field.

Execution:

Retrieve _internal events
    ->
Evaluate the status field
    ->
Keep events where status="error"
    ->
Count remaining events by host

Core distinction:

search
    ->
Search/refine events

where
    ->
Evaluate a field condition

---

## Question 3 - search vs stats

Example:

index=_internal
| search error

The result remains an event set.

Example:

index=_internal
| stats count by host

The result becomes a statistical table.

Core model:

search
    ->
FILTER

stats
    ->
TRANSFORM / AGGREGATE

---

## Question 4 - eval followed by stats

Example:

index=_internal
| eval severity=if(status="error","HIGH","NORMAL")
| stats count by severity

eval creates/calculates the severity field based on status.

Then stats counts the events by severity.

Execution:

Events
    ->
Create severity
    ->
Count by severity
    ->
Statistical table

Important distinction:

eval creates/calculates the field.

stats aggregates the results.

---

## Question 5 - eval, stats, and sort

Example:

index=_internal
| eval severity=if(status="error","HIGH","NORMAL")
| stats count by severity
| sort - count

If the original events are:

status=error
status=warning
status=error
status=info
status=error

eval produces:

HIGH
NORMAL
HIGH
NORMAL
HIGH

stats produces conceptually:

severity   count
HIGH       3
NORMAL     2

sort - count places the highest count first.

Result:

HIGH       3
NORMAL     2

---

## Question 6 - case() First True Condition

Example:

index=_internal
| eval severity=case(
    status="error", "HIGH",
    status="warning", "MEDIUM",
    true(), "LOW"
)

For:

status=warning

The result is:

severity=MEDIUM

Reason:

status="error"
    ->
FALSE

status="warning"
    ->
TRUE
    ->
MEDIUM

The true() fallback is not reached.

Core rule:

case()
    ->
First TRUE condition wins

---

## Question 7 - case() Fallback

For:

status=info

Using:

index=_internal
| eval severity=case(
    status="error", "HIGH",
    status="warning", "MEDIUM",
    true(), "LOW"
)

The result is:

severity=LOW

Reason:

status="error"
    ->
FALSE

status="warning"
    ->
FALSE

true()
    ->
TRUE
    ->
LOW

Core concept:

true() = fallback condition

---

## Question 8 - Pipeline Result Set

Example:

index=_internal
| search error
| eval severity="HIGH"
| stats count by severity

stats does not count all _internal events again.

Each command receives the result set produced by the previous command.

Execution:

All _internal events
    ->
search error
    ->
Only matching events survive
    ->
eval assigns severity=HIGH
    ->
stats counts those surviving events

Core rule:

Each command operates on the result set produced by the previous stage.

---

## Question 9 - where After stats

Example:

index=_internal
| stats count by host
| where count > 100

After stats, the result is a table containing host and count.

where now filters the statistical result rows.

Example:

host     count
PATEL    250
SERVER1  80
SERVER2  150

After:

| where count > 100

Result:

PATEL    250
SERVER2  150

Core distinction:

Before stats:

where
    ->
filters events

After stats:

where
    ->
filters result rows

---

## Question 10 - where Before vs After stats

Pipeline A:

index=_internal
| where status="error"
| stats count by host

where runs before stats.

It filters the original events based on the status field.

Execution:

Events
    ->
where status="error"
    ->
Only error events
    ->
stats count by host

Pipeline B:

index=_internal
| stats count by host
| where count > 100

where runs after stats.

It filters the statistical result rows based on count.

Execution:

Events
    ->
stats count by host
    ->
Host/count table
    ->
where count > 100
    ->
Only rows with count greater than 100

Core rule:

The same command can operate on different types of results depending on where it appears in the pipeline.

---

# Brainstorming Core Reasoning Model

Retrieve
    ->
Filter
    ->
Create / Calculate Fields
    ->
Aggregate / Transform
    ->
Filter Result Rows
    ->
Sort
    ->
Interpret

---

# Brainstorming Key Rules

search
    ->
FILTER / REFINE EVENTS

where
    ->
EVALUATE CONDITIONS AGAINST THE CURRENT RESULT SET

eval
    ->
CREATE / CALCULATE / MODIFY FIELDS

stats
    ->
AGGREGATE / TRANSFORM

sort
    ->
ORDER RESULTS

case()
    ->
FIRST TRUE CONDITION WINS

true()
    ->
FALLBACK CONDITION

---

# Brainstorming Checkpoint

Questions:

10 / 10

Strong Areas:

- SPL pipeline execution
- search filtering
- where filtering
- stats transformation
- eval field creation
- case() logic
- sort ordering
- result-set reasoning
- event filtering versus result-row filtering
- command execution order

Refinement Area:

- Be precise about whether a command is operating on raw events or statistical result rows.

---

# Module 02 Brainstorming Status

Theory:

COMPLETE

Brainstorming:

COMPLETE

Next:

Hands-On SPL Labs
    ->
Formal Questions
    ->
Scenarios
    ->
Troubleshooting
    ->
Weak-Area Identification
    ->
Retest
