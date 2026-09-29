# Module 05 — Stats and Visualization Notes

## Core Learning

Module 05 focused on understanding how Splunk transforms raw
events into statistical results and how those results can be
used for analysis and visualization.

The main focus was stats and how it works together with
eval, where, sort, and head.

## stats

stats is used to aggregate events and produce a reduced
result set.

Example:

    index=_internal
    | stats count by sourcetype

This produces one result row for each sourcetype.

The important mental model is:

    Many Events
        ->
    Aggregated Results

## Grouping

The BY clause determines how the aggregation is grouped.

Example:

    | stats count by host

Multiple fields can also be used.

Example:

    | stats count by host sourcetype

This groups the results by the combination of host and
sourcetype.

## Statistical Functions

### count

Counts events.

    | stats count by sourcetype

### dc()

Counts distinct values.

    | stats dc(host) as unique_hosts by sourcetype

### min()

Returns the minimum value.

    | stats min(_time) as earliest by sourcetype

### max()

Returns the maximum value.

    | stats max(_time) as latest by sourcetype

### avg()

Returns the average value.

    | stats avg(response_time) as average_response_time

## Multiple Statistics

Multiple statistics can be calculated in one stats command.

Example:

    | stats count as event_count dc(host) as unique_hosts by sourcetype

This allows multiple measurements to be produced in one result
set.

## eval + stats

eval can create a calculated field that is then aggregated
with stats.

Example:

    | eval kb_per_event=len(_raw)/1024
    | stats avg(kb_per_event) as avg_kb_per_event by sourcetype

The reasoning is:

    Raw Event
        ->
    eval
        ->
    Calculated Field
        ->
    stats
        ->
    Aggregated Result

## where + stats

where can be used after stats to filter the aggregated
results.

Example:

    | stats count by sourcetype
    | where count > 1000

Important distinction:

    stats
        ->
    aggregate
        ->
    where
        ->
    filter aggregated results

## sort

sort controls the order of the results.

Descending:

    | sort - count

Ascending:

    | sort count

For Top-N searches, descending sort is normally followed by
head.

## head

head limits the number of results.

Example:

    | sort - count
    | head 10

The important sequence is:

    stats
        ->
    sort
        ->
    head

This produces a Top-10 result.

## Visualization

Statistical results can be displayed using Splunk
visualizations.

For example:

    index=_internal
    | stats count by sourcetype
    | sort - count
    | head 10

The resulting statistics were displayed using a Column Chart.

The purpose of visualization is not to replace the statistics.

The visualization makes the relationship and differences in
the statistical results easier to recognize.

## Conditional Counting

count(eval(...)) allows conditional metrics to be calculated
inside stats.

Example:

    | stats count as total_events
        count(eval(sourcetype="splunkd")) as splunkd_events
        count(eval(sourcetype="splunkd_access")) as access_events

This allows multiple related measurements to be returned in
one result.

## Main Reasoning Model

When approaching a Stats and Visualization problem:

    What do I need to measure?
        ->
    What field should I group by?
        ->
    Which statistical function do I need?
        ->
    Do I need eval to calculate something?
        ->
    Do I need where to filter the result?
        ->
    Do I need sort to order the result?
        ->
    Do I need head to limit the result?
        ->
    Would a visualization make the result easier to understand?

## Important Distinctions

    eval  = create or calculate fields

    stats = aggregate and reduce events

    where = filter results

    sort  = order results

    head  = limit results

    dc()  = distinct count

    min() = minimum

    max() = maximum

    avg() = average

## Module 05 Learning Outcome

The major improvement from this module was understanding
the complete transformation pipeline rather than treating
each command independently.

    Raw Events
        ->
    Calculate
        ->
    Aggregate
        ->
    Filter
        ->
    Sort
        ->
    Limit
        ->
    Visualize

Module 05 theory, brainstorming, question practice, and
hands-on laboratory work are complete.
