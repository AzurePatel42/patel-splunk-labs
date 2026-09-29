# Module 06 — Search and Reporting Scenarios

## Scenario 01 — Find Error Events

### Situation
An analyst needs to quickly find events containing the term
error in the internal Splunk index.

### SPL

    index=_internal error

### Reasoning

    Need matching event content
        ->
    Search for error
        ->
    Return matching events

### Key Learning

A basic search term can be used to locate matching event
content.


## Scenario 02 — Search for Error or Warning Events

### Situation
An analyst needs to find events containing either error or
warn.

### SPL

    index=_internal error OR warn

### Reasoning

    Either condition is acceptable
        ->
    Use OR

### Key Learning

OR is used when either search condition can match.


## Scenario 03 — Find Errors from splunkd

### Situation
An analyst wants only error events associated with the
splunkd sourcetype.

### SPL

    index=_internal sourcetype=splunkd error

### Reasoning

    Need specific sourcetype
        ->
    sourcetype=splunkd

    Need error events
        ->
    error

### Key Learning

Field-value searches can be combined with event-content
search terms to narrow results.


## Scenario 04 — Restrict the Search to One Hour

### Situation
An analyst needs to investigate recent splunkd activity and
only wants events from the last hour.

### SPL

    index=_internal sourcetype=splunkd earliest=-1h latest=now

### Reasoning

    Need recent activity
        ->
    Define time boundaries
        ->
    earliest=-1h
    latest=now

### Key Learning

Explicit time modifiers control the search window.


## Scenario 05 — Find Events Matching Both Conditions

### Situation
An analyst needs splunkd events where both error and warn
are present.

### SPL

    index=_internal sourcetype=splunkd error AND warn

### Reasoning

    Both conditions required
        ->
    Use AND

### Key Learning

AND requires both conditions to match.


## Scenario 06 — Exclude Error Events

### Situation
An analyst wants splunkd events but does not want events
containing error.

### SPL

    index=_internal sourcetype=splunkd NOT error

### Reasoning

    Need splunkd events
        ->
    sourcetype=splunkd

    Need to exclude error
        ->
    NOT error

### Key Learning

NOT removes events matching the excluded condition.


## Scenario 07 — Search Multiple Related Sourcetypes

### Situation
An analyst needs events from sourcetypes that begin with

ode:sidecar: but does not need to specify each sourcetype
individually.

### SPL

    index=_internal sourcetype=node:sidecar:*

### Reasoning

    Common sourcetype pattern
        ->
    Use wildcard

### Key Learning

Wildcards allow a search to match multiple related field
values.


## Scenario 08 — Return Only Required Fields

### Situation
An analyst is investigating splunkd events but only needs
the timestamp, host, and sourcetype fields.

### SPL

    index=_internal sourcetype=splunkd
    | fields _time host sourcetype

### Reasoning

    Need specific fields
        ->
    fields

### Key Learning

The fields command controls which fields are retained in the
result.


## Scenario 09 — Create a Tabular Investigation View

### Situation
An analyst wants a simple table containing timestamp, host,
sourcetype, and source.

### SPL

    index=_internal sourcetype=splunkd
    | table _time host sourcetype source

### Reasoning

    Need selected fields
        ->
    Need tabular presentation
        ->
    table

### Key Learning

The table command produces a tabular result using the
specified fields.


## Scenario 10 — Search Construction Decision Model

### Situation
An analyst receives an unfamiliar search requirement and must
construct the search without guessing.

### Reasoning

    What events do I need?
        ->
    Choose the index

    What identifies the events?
        ->
    Search term or field=value

    Do I need multiple conditions?
        ->
    AND / OR / NOT

    Is there a field-value pattern?
        ->
    Wildcard

    What time period is required?
        ->
    earliest / latest

    Which fields are needed?
        ->
    fields

    Is a table required?
        ->
    table

### Key Learning

Search construction should follow the requirement step by
step rather than selecting commands randomly.


# Module 06 Scenario Summary

| Scenario | Topic | Status |
|---:|---|:---:|
| 01 | Find error events | Complete |
| 02 | Error or warning events | Complete |
| 03 | Filter by sourcetype | Complete |
| 04 | Time-restricted search | Complete |
| 05 | AND conditions | Complete |
| 06 | NOT exclusion | Complete |
| 07 | Wildcard search | Complete |
| 08 | Field selection | Complete |
| 09 | Tabular reporting | Complete |
| 10 | Search construction model | Complete |

## Final Status

Module 06 — Search and Reporting: Documentation Complete
