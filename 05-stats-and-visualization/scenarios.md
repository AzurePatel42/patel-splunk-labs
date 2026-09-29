# Module 05 — Stats and Visualization Scenarios

## Scenario Practice Status

Module 05 scenario practice was completed as part of the
theory and brainstorming work.

The scenarios focused on recognizing when to use statistical
aggregation, calculated fields, filtering, sorting, limiting
results, and visualization.

## Scenario 01 — Top 10 Sourcetypes

### Requirement

Identify the ten sourcetypes producing the most events.

### Solution

    index=_internal
    | stats count by sourcetype
    | sort - count
    | head 10

### Reasoning

    Need event counts
        ->
    stats count by sourcetype

    Need highest counts first
        ->
    sort - count

    Need only ten results
        ->
    head 10

### Key Learning

The order of the commands matters.

    stats
        ->
    sort
        ->
    head

---

## Scenario 02 — Top 5 Sourcetypes

### Requirement

Identify the five sourcetypes producing the most events and
display them as a visualization.

### Solution

    index=_internal
    | stats count by sourcetype
    | sort - count
    | head 5

### Visualization

    Column Chart

### Reasoning

The statistical result is created first and then limited to
the five highest values.

### Key Learning

A statistical result can be converted into a visualization
after the result set has been prepared.

---

## Scenario 03 — High-Volume Sourcetypes

### Requirement

Identify sourcetypes producing more than 1,000 events.

### Solution

    index=_internal
    | stats count by sourcetype
    | where count > 1000
    | sort - count

### Reasoning

The condition applies to the aggregated count.

    events
        ->
    stats
        ->
    where
        ->
    sort

### Key Learning

where can filter the results produced by stats.

---

## Scenario 04 — Events by Host

### Requirement

Determine which hosts are generating the most events.

### Solution

    index=_internal
    | stats count by host
    | sort - count
    | head 10

### Lab Result

The local Splunk environment returned one host:

    PATEL

### Key Learning

The field used after BY determines the dimension being
measured.

---

## Scenario 05 — Host and Sourcetype

### Requirement

Determine event volume by both host and sourcetype.

### Solution

    index=_internal
    | stats count by host sourcetype
    | sort - count
    | head 10

### Reasoning

The aggregation uses two fields.

    host + sourcetype
        ->
    grouped result

### Key Learning

Multiple fields can be used to create a more detailed
aggregation.

---

## Scenario 06 — Average Event Size

### Requirement

Calculate the approximate average event size for each
sourcetype.

### Solution

    index=_internal
    | eval kb_per_event=len(_raw)/1024
    | stats avg(kb_per_event) as avg_kb_per_event by sourcetype
    | sort - avg_kb_per_event
    | head 10

### Reasoning

The event size must first be calculated.

    raw event
        ->
    eval
        ->
    kb_per_event
        ->
    stats avg()
        ->
    sourcetype result

### Key Learning

eval can create a field that is then aggregated with
stats.

---

## Scenario 07 — Earliest and Latest Events

### Requirement

Determine the earliest and latest event timestamps for each
sourcetype and calculate the time span.

### Solution

    index=_internal
    | stats min(_time) as earliest max(_time) as latest count by sourcetype
    | eval duration_seconds=latest-earliest
    | sort - duration_seconds

### Reasoning

    min(_time)
        ->
    earliest event

    max(_time)
        ->
    latest event

    latest - earliest
        ->
    duration

### Key Learning

The timestamps can be aggregated first and then used by
eval for additional calculations.

---

## Scenario 08 — Conditional Metrics

### Requirement

Return total events, splunkd events, and splunkd_access
events in one result.

### Solution

    index=_internal
    | stats count as total_events
        count(eval(sourcetype="splunkd")) as splunkd_events
        count(eval(sourcetype="splunkd_access")) as access_events

### Reasoning

The three metrics can be calculated in the same stats
command.

### Key Learning

count(eval(...)) provides conditional counting.

---

## Scenario 09 — Multiple Statistical Measurements

### Requirement

Return event count and the number of unique hosts for each
sourcetype.

### Solution

    index=_internal
    | stats count as event_count dc(host) as unique_hosts by sourcetype
    | sort - event_count

### Reasoning

Two different measurements are required:

    count
        ->
    total events

    dc(host)
        ->
    unique hosts

### Key Learning

Multiple statistical functions can be combined in one
stats command.

---

## Scenario 10 — Operational Visualization

### Requirement

Create a report that makes the highest-volume sourcetypes
easy to compare visually.

### Solution

    index=_internal
    | stats count by sourcetype
    | sort - count
    | head 10

### Visualization

    Column Chart

### Reasoning

The search creates a Top-10 statistical result first.
The Column Chart then provides a visual comparison of event
volume.

### Key Learning

Visualization is the final presentation layer of the
statistical result.

---

## Scenario Reasoning Model

When given a Stats and Visualization scenario:

    Need to measure something?
        ->
    stats

    Need to calculate a field?
        ->
    eval

    Need to filter the result?
        ->
    where

    Need to order the result?
        ->
    sort

    Need only Top-N results?
        ->
    head

    Need to show the result visually?
        ->
    Visualization

## Scenario Practice Completion

- Theory: COMPLETE
- Brainstorming: COMPLETE
- Question practice: COMPLETE
- Scenario practice: COMPLETE
- Hands-on labs: COMPLETE
- Documentation: COMPLETE

## Module 05 Final Status

Module 05 — Stats and Visualization

    Theory              COMPLETE
    Brainstorming       COMPLETE
    Questions           COMPLETE
    Scenarios           COMPLETE
    Labs                10/10 COMPLETE
    Documentation       COMPLETE

The module is ready for Git commit and push.
