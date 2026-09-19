# Splunk Core - 01 Search Basics Labs

## Lab Objective

Practice retrieving and refining events using basic SPL.

Use the Splunk Search & Reporting interface.

The examples below use the `_internal` index when available.

---

# Lab 1 - Search an Index

Run:

index=_internal

Observe:

- Events returned
- Time range
- Host
- Source
- Sourcetype

---

# Lab 2 - Keyword Search

Run:

index=_internal error

Observe:

- How the result set changes
- Which events contain the keyword
- Whether the keyword appears in raw event text

---

# Lab 3 - Phrase Search

Run:

index=_internal "connection refused"

Compare with:

index=_internal connection refused

Record the difference in returned events.

---

# Lab 4 - Field Search

Run:

index=_internal host=*

Then identify one host from the results and search:

index=_internal host=<your-host>

---

# Lab 5 - Boolean Search

Run:

index=_internal error OR warning

Then:

index=_internal error AND warning

Then:

index=_internal NOT debug

Compare the result sets.

---

# Lab 6 - Wildcard Search

Run:

index=_internal host=*

If multiple hosts exist, use a prefix wildcard based on an actual host:

index=_internal host=<prefix>*

Record why the wildcard is useful.

---

# Lab 7 - Time Range

Run:

index=_internal earliest=-1h latest=now

Then compare with:

index=_internal earliest=-24h latest=now

Record how the time range changes the result set.

---

# Lab 8 - Search Pipeline

Run:

index=_internal
| search error

Compare with:

index=_internal error

Explain why both can return similar results.

---

# Lab 9 - Progressive Refinement

Start with:

index=_internal

Refine to:

index=_internal error

Then:

index=_internal error earliest=-1h

Then add a host filter based on evidence from the previous search.

Document the reasoning.

---

# Lab 10 - Evidence-Based Search

Start broad.

Reduce the result set using:

1. Index
2. Keyword or phrase
3. Field
4. Time
5. Additional pipeline processing

Do not add filters without a reason.

---

# Lab Completion Standard

The lab is complete when you can explain:

- Why the index is specified
- What the search terms match
- Why quotes change phrase behavior
- What field=value means
- How Boolean logic changes results
- Why time range matters
- What the pipe does
- How to refine a search from evidence

---

# September 19, 2026 - Search Basics Reasoning Session

Status:

SPL construction and reasoning practiced.

Formal questions:

10 / 10 complete

Formal score:

94%

Live Splunk execution:

NOT YET RECORDED

Reason:

The session focused on learning SPL logic and constructing searches from
requirements before recording hands-on execution results.

Next lab objective:

Execute Labs 1-10 in Splunk Search & Reporting and record the observed
results.

---

# Key Learning From Session

SQL-style reasoning can be used to understand Splunk data operations:

Filter
  ->
Group
  ->
Aggregate
  ->
Sort
  ->
Limit

Splunk-specific commands then express those operations.

Important distinction:

stats summarizes events.

The original events may require a separate retrieval strategy after
aggregation.

Troubleshooting principle:

Use evidence before changing the search.

Change one condition at a time and observe the result.

---

# Lab 1 - Actual Execution Result

## Search

index=_internal

## Splunk Result

Events returned:

10,507

Time range displayed:

September 18, 2026 3:00:00 PM
to
September 19, 2026 3:06:30 PM

Event sampling:

No Event Sampling

## Observed Fields

Selected fields included:

- ahost
- asource
- asourcetype

Interesting fields included:

- acomponent
- aevent_message
- aeventtype
- agroup
- aindex
- alog_level
- aname
- asplunk_server

## Observation

The search successfully retrieved indexed events from the `_internal`
index.

The result set contained 10,507 events and Splunk reported that no event
sampling was being used.

## Learning

index=_internal

means:

Search the `_internal` index and return matching indexed events.

Lab 1 status:

COMPLETE

---

# Lab 2 - Actual Execution Result

## Search

index=_internal error

## Comparison With Lab 1

Lab 1:

index=_internal

Events:

10,507

Lab 2:

index=_internal error

Events:

148

## Event Sampling

No Event Sampling

## Observed Behavior

Adding the keyword `error` reduced the result set from 10,507 events to
148 events.

The returned events included error-related messages from Splunk internal
components and services.

Observed examples included:

- Malformed JWT string
- PostgreSQL SSL-related error
- HTTP 401 authentication error
- HTTP 404 configuration-related messages

Observed fields included:

- host
- source
- sourcetype
- error
- event_message
- log_level
- message

## Learning

The keyword `error` acts as a search condition against the indexed event
data.

The search did not change the index.

The search changed the event set returned from the same index.

Mental model:

index=_internal
  ->
10,507 events

index=_internal error
  ->
148 matching events

Lab 2 status:

COMPLETE
---
# Lab 3 - Actual Execution Result

Search executed:
index=_internal "connection refused"

Time range:
Last 24 hours

Actual result:
0 events

Observed result:
The search completed successfully, but no events contained the exact phrase "connection refused" during the selected time range.

Interpretation:
The index contains data, but the exact phrase searched for was not found in the current 24-hour window.

Learning:
A zero-result search is still a valid search result. It tells us that the specific condition was not found. We should troubleshoot methodically rather than assume the search itself is broken.

Troubleshooting lesson:
Verify the index first, then the time range, then the search term or phrase, then fields and values. Change one condition at a time.

Lab 3 status:
COMPLETE
---
# Lab 4 - Actual Execution Result

Search executed:
index=_internal host=PATEL

Time range:
Last 24 hours

Actual result:
12,101 events

Event sampling:
No Event Sampling

Observed host:
PATEL

Observed sources:
C:\Program Files\Splunk\var\log\splunk\splunkd_ui_access.log
C:\Program Files\Splunk\var\log\splunk\health.log

Observed sourcetypes:
splunkd_ui_access
splunkd

Interpretation:
The host=PATEL field filter returned events associated with the PATEL host.

Learning:
Field-value searches allow us to target events associated with a specific field value. The host field identifies the originating host associated with the event.

Important comparison lesson:
Event counts from different searches should only be compared carefully when the searches use equivalent time windows and are executed at comparable times. Splunk internal logs continue to generate new events.

Lab 4 status:
COMPLETE
---
# Lab 5 - Actual Execution Result

Search executed:
index=_internal status=*

Time range:
Last 24 hours

Actual result:
995 events

Event sampling:
No Event Sampling

Observed field:
status

Observed Splunk field display:
#status

Interpretation:
The search returned events in which the status field is present and contains a value.

Learning:
The wildcard in field=value searches can be used to match events where the specified field exists.

Important distinction:
status=* does not mean "show all possible status values." It means search for events where the status field has a value.

Lab 5 status:
COMPLETE
---
# Lab 6 - Actual Execution Result

Search executed:
index=_internal error OR warning

Time range:
Last 24 hours

Actual result:
179 events

Event sampling:
No Event Sampling

Observed result:
The search returned events matching either the keyword error or the keyword warning.

Observed examples:
- log_level=WARN
- error text including "pq: SSL is not enabled on the server"
- Other Splunk internal warning/error messages

Interpretation:
OR broadens a keyword search by allowing either condition to match.

Learning:
Boolean OR is useful when multiple alternative conditions should be included in the same search.

Mental model:
condition A OR condition B
= return events matching A or matching B

Lab 6 status:
COMPLETE
---
# Lab 7 - Actual Execution Result

Search executed:
index=_internal earliest=-1h latest=now

Time picker:
Last 24 hours

SPL time range:
earliest=-1h latest=now

Actual result:
13,154 events

Effective event time range:
9/19/26 2:13:37 PM to 9/19/26 3:13:37 PM

Event sampling:
No Event Sampling

Interpretation:
The explicit earliest and latest time modifiers in the SPL limited the search to the last one hour, despite the Splunk time picker remaining set to Last 24 hours.

Learning:
Time boundaries can be specified directly in SPL using earliest and latest.

Mental model:
Time picker + SPL time modifiers
-> the search operates on the resulting effective time constraints.

Lab 7 status:
COMPLETE
---
# Lab 8 - Actual Execution Result

Search executed:
index=_internal
| stats count by host

Time range:
Last 24 hours

Input events:
13,287 events

Statistics result:
1 row

Observed group:
host = PATEL

Count column:
The captured search output did not visibly include the numeric count value, so no count value is recorded here.

Interpretation:
The stats command transformed the event results into a grouped statistical result using host as the grouping field.

Learning:
stats count by host is conceptually similar to a SQL GROUP BY host with COUNT(*).

Mental model:
Raw events
-> group by host
-> count events in each host group
-> return a statistics table

Lab 8 status:
COMPLETE
---
# Lab 9 - Actual Execution Result

Search executed:
index=_internal
| stats count by sourcetype

Time range:
Last 24 hours

Input events:
13,904 events

Statistics result:
21 rows

Observed statistics:
sourcetype -> count

Visible results included:
- mongod = 9
- node:sidecar:agent_manager:stdout = 36
- node:sidecar:identity:stdout = 72
- node:sidecar:ipc_broker:stdout = 66
- node:sidecar:postgres:stderr = 105
- node:sidecar:postgres:stdout = 680
- node:sidecar:spotlight_collector:stderr = 16
- node:sidecar:spotlight_collector:stdout = 9
- node:sidecar:topology:stdout = 5
- node:supervisor = 478
- scheduler = 10
- splunk_first_install = 1
- splunk_o11y_app = 1
- splunk_search_messages = 2
- splunk_version = 1
- splunk_web_access = 45
- splunk_web_service = 319
- splunkd = 10882
- splunkd_access = 627
- splunkd_conf = 1

Interpretation:
The stats command grouped the events by sourcetype and counted the events in each group.

Learning:
The field following "by" determines the grouping dimension.

Comparison:
stats count by host
-> event counts grouped by host

stats count by sourcetype
-> event counts grouped by sourcetype

SQL mental model:
GROUP BY sourcetype
COUNT(*)

Lab 9 status:
COMPLETE
---
# Lab 10 - Actual Execution Result

Search executed:
index=_internal host=PATEL earliest=-1h latest=now
| stats count by sourcetype

Time picker:
Last 24 hours

SPL time range:
earliest=-1h latest=now

Effective event time range:
9/19/26 2:16:01 PM to 9/19/26 3:16:01 PM

Input events:
14,203 events

Statistics result:
21 rows

Observed statistics:
sourcetype -> count

Visible examples:
- mongod = 9
- node:sidecar:agent_manager:stdout = 37
- node:sidecar:identity:stdout = 74
- node:sidecar:ipc_broker:stdout = 66
- node:sidecar:postgres:stderr = 105
- node:sidecar:postgres:stdout = 693
- node:sidecar:spotlight_collector:stderr = 16
- node:sidecar:spotlight_collector:stdout = 9
- node:sidecar:topology:stdout = 5
- node:supervisor = 478
- scheduler = 10
- splunk_first_install = 1
- splunk_o11y_app = 1
- splunk_search_messages = 2
- splunk_version = 1
- splunk_web_access = 45
- splunk_web_service = 319
- splunkd = 11113
- splunkd_access = 636
- splunkd_conf = 1

Interpretation:
The search combined an index scope, host filter, explicit one-hour time range, and stats aggregation.

Learning:
Multiple search conditions can be combined to progressively narrow the event set before transforming the results with a reporting command.

Mental model:
Scope
-> Filter
-> Time
-> Transform
-> Interpret

SQL mental model:
WHERE host = 'PATEL'
AND time is within the selected hour
GROUP BY sourcetype
COUNT(*)

Lab 10 status:
COMPLETE
