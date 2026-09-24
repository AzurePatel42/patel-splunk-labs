# Splunk Core - 02 SPL Fundamentals

# Hands-On SPL Labs

## Lab 1 - SPL Pipeline: search -> stats -> sort

### Objective

Build and execute an SPL pipeline that:

Retrieve events from the _internal index
    ->
Filter events containing error
    ->
Count matching events by host
    ->
Sort results by count in descending order

---

### SPL

index=_internal
| search error
| stats count by host
| sort - count

---

### Execution Result

Search completed successfully.

Time range displayed:

9/23/26 3:00:00 AM
to
9/24/26 3:53:14 AM

Events:

763

Statistics result:

PATEL

The Statistics view displayed one result row because the matching events belonged to one host:

PATEL

---

### Result Interpretation

There were 763 matching events.

However, stats count by host does not display one row for every event.

It groups the events by host.

Because the matching events belonged to only one host:

PATEL

the statistical result contained one host group.

Core model:

763 events
    ->
stats count by host
    ->
1 host group
    ->
PATEL

---

### Key Learning

The number of raw events does not necessarily equal the number of rows returned by stats.

Raw events:

763

Distinct host groups:

1

The final result is a statistical table rather than the original raw event stream.

---

### Troubleshooting / Verification

Search completed successfully.

No event sampling was reported.

The Statistics tab was selected.

The result showed host-based aggregation.

---

### Lab Status

COMPLETE

Understanding:

10 / 10

---

## Module 02 Lab Progress

Lab 1:

COMPLETE

Next:

Lab 2

---

## Lab 2 - where vs search

### Objective

Compare event filtering with:

search error

versus field-based filtering with:

where status="error"

---

### SPL

index=_internal
| where status="error"
| stats count by host
| sort - count

---

### Execution Result

Search completed successfully.

Time range displayed:

9/23/26 3:00:00 AM
to
9/24/26 3:57:46 AM

Events:

0

Statistics results:

0

No results were found.

---

### Result Interpretation

The search returned zero events where:

status="error"

This demonstrates that the current _internal data did not contain events matching that specific field condition during the displayed time range.

---

### Comparison With Lab 1

Lab 1 used:

index=_internal
| search error
| stats count by host
| sort - count

Result:

763 matching events

Lab 2 used:

index=_internal
| where status="error"
| stats count by host
| sort - count

Result:

0 matching events

---

### Core Distinction

search error

Searches the current event set for the search term:

error

where status="error"

Evaluates the specific status field and keeps only events where:

status = error

Core model:

search error
    ->
Search/refine events using a search expression

where status="error"
    ->
Evaluate a specific field condition

---

### Key Learning

A search term and a field condition are not automatically equivalent.

The presence of the word:

error

in an event does not prove that:

status="error"

---

### Troubleshooting / Verification

Search completed successfully.

No event sampling was reported.

The Statistics tab was selected.

No matching events were found.

The result was consistent with the field-based condition.

---

### Lab Status

COMPLETE

Understanding:

10 / 10

---

## Module 02 Lab Progress

Lab 1:

COMPLETE

Lab 2:

COMPLETE

Next:

Lab 3

---

## Lab 3 - eval Creates a Field

### Objective

Use eval to create a new field and then use stats to count events by that field.

---

### SPL

index=_internal
| search error
| eval severity="HIGH"
| stats count by severity

---

### Execution Result

Search completed successfully.

Events:

774

Statistics result:

severity   count
HIGH       774

Statistics rows:

1

---

### Result Interpretation

The search returned 774 events matching the search term:

error

The eval command created a new field:

severity

Each surviving event received:

severity=HIGH

The stats command then grouped the events by severity and counted them.

Because every surviving event had:

severity=HIGH

there was only one statistical group:

HIGH = 774

---

### Execution Flow

index=_internal
    ->
search error
    ->
774 matching events
    ->
eval severity="HIGH"
    ->
Each event receives severity=HIGH
    ->
stats count by severity
    ->
HIGH = 774

---

### Key Learning

eval can create a new field for the current result set.

stats count by severity groups events according to the value of the severity field.

The number of raw events does not necessarily equal the number of statistical rows.

In this lab:

Raw events:

774

Statistical rows:

1

Reason:

All 774 events had the same severity value:

HIGH

---

### Troubleshooting / Verification

Search completed successfully.

No event sampling was reported.

The Statistics tab was selected.

The result contained one severity group.

The count for HIGH was 774.

---

### Lab Status

COMPLETE

Understanding:

10 / 10

---

## Module 02 Lab Progress

Lab 1:

COMPLETE

Lab 2:

COMPLETE

Lab 3:

COMPLETE

Next:

Lab 4

---

## Lab 4 - Multi-Field Grouping

### Objective

Use stats to count events by both host and sourcetype.

---

### SPL

index=_internal
| search error
| stats count by host sourcetype
| sort - count

---

### Execution Result

Search completed successfully.

Events:

790

Statistics rows:

10

The host was:

PATEL

The search returned 10 different sourcetypes.

---

### Result Interpretation

The search returned 790 events matching the search term:

error

The stats command grouped the events by both:

host

and:

sourcetype

Although there was only one host:

PATEL

there were 10 different sourcetypes.

Therefore, Splunk produced 10 statistical rows.

The result was one row for each unique:

host + sourcetype

combination.

---

### Execution Flow

index=_internal
    ->
search error
    ->
790 matching events
    ->
stats count by host sourcetype
    ->
Events grouped by host + sourcetype
    ->
10 statistical rows
    ->
sort - count

---

### Key Learning

stats count by multiple fields groups events according to the combination of those fields.

A single host can produce multiple statistical rows when multiple sourcetypes exist.

In this lab:

Raw events:

790

Host:

PATEL

Sourcetypes:

10

Statistical rows:

10

Reason:

The statistics were grouped by:

host + sourcetype

---

### Troubleshooting / Verification

Search completed successfully.

No event sampling was reported.

The Statistics tab was selected.

The result contained 10 statistical rows.

All rows had the same host:

PATEL

The rows represented 10 different sourcetypes.

---

### Lab Status

COMPLETE

Understanding:

10 / 10

---

## Module 02 Lab Progress

Lab 1:

COMPLETE

Lab 2:

COMPLETE

Lab 3:

COMPLETE

Lab 4:

COMPLETE

Next:

Lab 5

---

## Lab 5 - eval + stats with Multiple Values

### Objective

Use eval to create a severity field with multiple possible values and then use stats to count events by severity.

---

### SPL

index=_internal
| search error
| eval severity=if(match(_raw,"error"),"HIGH","NORMAL")
| stats count by severity
| sort - count

---

### Execution Result

Search completed successfully.

Events:

803

Statistics result:

severity   count
HIGH       679
NORMAL     124

Statistics rows:

2

---

### Result Interpretation

The search returned 803 events matching the search term:

error

The eval command created a new field:

severity

The eval expression assigned one of two possible values:

HIGH

or:

NORMAL

The stats command then grouped the events by severity and counted them.

The result contained two severity groups:

HIGH = 679

NORMAL = 124

The two groups accounted for all 803 events.

---

### Execution Flow

index=_internal
    ->
search error
    ->
803 matching events
    ->
eval severity=if(match(_raw,"error"),"HIGH","NORMAL")
    ->
Events receive HIGH or NORMAL
    ->
stats count by severity
    ->
HIGH = 679
    ->
NORMAL = 124
    ->
2 statistical rows

---

### Key Learning

eval can create a field with multiple possible values.

stats count by severity groups events according to the values of the severity field.

The number of raw events does not necessarily equal the number of statistical rows.

In this lab:

Raw events:

803

Severity groups:

2

Statistical rows:

2

The two severity values were:

HIGH

NORMAL

---

### Troubleshooting / Verification

Search completed successfully.

No event sampling was reported.

The Statistics tab was selected.

The result contained two severity groups.

HIGH contained 679 events.

NORMAL contained 124 events.

679 + 124 = 803

All matching events were accounted for.

---

### Lab Status

COMPLETE

Understanding:

10 / 10

---

## Module 02 Lab Progress

Lab 1:

COMPLETE

Lab 2:

COMPLETE

Lab 3:

COMPLETE

Lab 4:

COMPLETE

Lab 5:

COMPLETE

Next:

Lab 6

---

## Lab 6 - where After stats

### Objective

Use stats to create statistical result rows and then use where to filter those result rows based on the count field.

---

### SPL

index=_internal
| search error
| stats count by sourcetype
| where count > 20
| sort - count

---

### Execution Result

Search completed successfully.

Events:

839

Statistics rows:

5

The Statistics result contained five sourcetypes with counts greater than:

20

Results:

node:sidecar:postgres:stdout = 280

splunkd = 267

node:supervisor = 131

splunkd_ui_access = 77

postgres = 49

---

### Result Interpretation

The search returned 839 events matching the search term:

error

The stats command grouped the events by:

sourcetype

and created a count field for each sourcetype.

The where command then evaluated:

count > 20

The where command operated on the statistical result rows created by stats.

Only sourcetypes with counts greater than 20 remained.

The result contained five statistical rows.

---

### Execution Flow

index=_internal
    ->
search error
    ->
839 matching events
    ->
stats count by sourcetype
    ->
Sourcetype/count result rows
    ->
where count > 20
    ->
5 remaining rows
    ->
sort - count

---

### Key Learning

where can filter statistical result rows after stats has transformed the event stream.

Before stats:

The data consists of raw events.

After stats:

The data consists of statistical result rows containing fields such as:

sourcetype

count

Therefore:

where count > 20

filters the count field in the statistical result set.

---

### Troubleshooting / Verification

Search completed successfully.

No event sampling was reported.

The Statistics tab was selected.

The result contained five statistical rows.

All remaining count values were greater than 20.

The smallest remaining count was:

49

The largest remaining count was:

280

---

### Lab Status

COMPLETE

Understanding:

10 / 10

---

## Module 02 Lab Progress

Lab 1:

COMPLETE

Lab 2:

COMPLETE

Lab 3:

COMPLETE

Lab 4:

COMPLETE

Lab 5:

COMPLETE

Lab 6:

COMPLETE

Next:

Lab 7

---

## Lab 7 - where Before stats

### Objective

Use where to filter raw events before stats and then count the remaining events by sourcetype.

---

### SPL

index=_internal
| search error
| where like(sourcetype,"%postgres%")
| stats count by sourcetype
| sort - count

---

### Execution Result

Search completed successfully.

Events:

329

Statistics rows:

2

Statistics result:

sourcetype   count
node:sidecar:postgres:stdout   280
postgres   49

---

### Result Interpretation

The search returned 329 events after the where filter.

The where command evaluated the sourcetype field before stats executed.

The filter:

where like(sourcetype,"%postgres%")

kept events whose sourcetype contained:

postgres

The stats command then grouped the remaining events by sourcetype.

The result contained two statistical rows:

node:sidecar:postgres:stdout = 280

postgres = 49

The two groups accounted for all 329 events.

---

### Execution Flow

index=_internal
    ->
search error
    ->
where sourcetype contains "postgres"
    ->
329 matching events
    ->
stats count by sourcetype
    ->
2 statistical rows
    ->
sort - count

---

### Key Learning

The position of where in the SPL pipeline determines what it filters.

When where appears before stats:

where filters the raw events.

When where appears after stats:

where filters the statistical result rows.

In this lab:

where

came before:

stats

Therefore, the sourcetype condition was applied to the raw events before aggregation.

---

### Troubleshooting / Verification

Search completed successfully.

No event sampling was reported.

The Statistics tab was selected.

The result contained two statistical rows.

The node:sidecar:postgres:stdout group contained 280 events.

The postgres group contained 49 events.

280 + 49 = 329

All filtered events were accounted for.

---

### Lab Status

COMPLETE

Understanding:

10 / 10

---

## Module 02 Lab Progress

Lab 1:

COMPLETE

Lab 2:

COMPLETE

Lab 3:

COMPLETE

Lab 4:

COMPLETE

Lab 5:

COMPLETE

Lab 6:

COMPLETE

Lab 7:

COMPLETE

Next:

Lab 8

---

## Lab 8 - Multiple stats Aggregations

### Objective

Use multiple stats aggregation functions to calculate count, average, maximum, and minimum values by sourcetype.

---

### SPL

index=_internal
| search error
| stats count avg(linecount) max(linecount) min(linecount) by sourcetype
| sort - count

---

### Execution Result

Search completed successfully.

Events:

893

Statistics rows:

10

The results contained the following aggregation fields:

count

avg(linecount)

max(linecount)

min(linecount)

Example:

node:supervisor

count:

131

avg(linecount):

1.0076335877862594

max(linecount):

2

min(linecount):

1

---

### Result Interpretation

The search returned 893 events matching the search term:

error

The stats command grouped the events by:

sourcetype

For each sourcetype, stats calculated:

count

The number of events in the sourcetype group.

avg(linecount)

The average linecount value within the sourcetype group.

max(linecount)

The highest linecount value within the sourcetype group.

min(linecount)

The lowest linecount value within the sourcetype group.

For node:supervisor:

count = 131

avg(linecount) = 1.0076335877862594

max(linecount) = 2

min(linecount) = 1

The max value of 2 means that at least one node:supervisor event had a linecount value of 2.

---

### Execution Flow

index=_internal
    ->
search error
    ->
893 matching events
    ->
stats count avg(linecount) max(linecount) min(linecount) by sourcetype
    ->
10 sourcetype groups
    ->
Multiple aggregations calculated for each group
    ->
sort - count

---

### Key Learning

stats can perform multiple aggregation functions in the same command.

count measures the number of events in each group.

avg calculates the average value of a field.

max identifies the highest value of a field.

min identifies the lowest value of a field.

The aggregation functions operate within each sourcetype group.

The number of events is represented by count, not max or min.

---

### Troubleshooting / Verification

Search completed successfully.

No event sampling was reported.

The Statistics tab was selected.

The result contained 10 statistical rows.

The node:supervisor group contained 131 events.

The maximum linecount for node:supervisor was 2.

The minimum linecount for node:supervisor was 1.

---

### Lab Status

COMPLETE

Understanding:

10 / 10

---

## Module 02 Lab Progress

Lab 1:

COMPLETE

Lab 2:

COMPLETE

Lab 3:

COMPLETE

Lab 4:

COMPLETE

Lab 5:

COMPLETE

Lab 6:

COMPLETE

Lab 7:

COMPLETE

Lab 8:

COMPLETE

Next:

Lab 9
