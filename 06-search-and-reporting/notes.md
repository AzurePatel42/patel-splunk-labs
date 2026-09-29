# Module 06 — Search and Reporting Notes

## Core Learning

Module 06 focused on using Splunk search to locate, filter, and
control indexed events and to understand how search conditions
change the result set.

The main focus was basic search syntax, field-value searches,
Boolean operators, wildcards, time modifiers, and commands used
to control returned fields.

## Basic Search

A basic search term searches event content.

Example:

    index=_internal error

This returns events containing the search term.

The important mental model is:

    Indexed Events
        ->
    Search Term
        ->
    Matching Events

## Field-Value Search

A field-value search narrows the search using a specific field.

Example:

    index=_internal sourcetype=splunkd error

The search requires the sourcetype field to match splunkd
while also searching for the term error.

The important distinction is:

    field=value
        ->
    targeted filtering

## Boolean Operators

### OR

OR allows either condition to match.

Example:

    index=_internal error OR warn

The search can return events matching error or warn.

Important distinction:

    OR = either condition

### AND

AND requires both conditions to match.

Example:

    index=_internal sourcetype=splunkd error AND warn

Important distinction:

    AND = both conditions

### NOT

NOT excludes matching events.

Example:

    index=_internal sourcetype=splunkd NOT error

Important distinction:

    NOT = exclude condition

## Wildcards

Wildcards can be used when the exact field value is not known
but the value follows a common pattern.

Example:

    index=_internal sourcetype=node:sidecar:*

This matches sourcetypes beginning with the specified pattern.

The important mental model is:

    Common prefix
        ->
    wildcard
        ->
    multiple matching values

## Time Modifiers

Searches can specify an explicit time range.

Example:

    index=_internal sourcetype=splunkd earliest=-1h latest=now

This restricts the search to the last hour.

Important distinction:

    earliest = beginning of search window

    latest = end of search window

Time constraints can significantly reduce the search scope.

## fields

The fields command controls which fields are retained.

Example:

    index=_internal sourcetype=splunkd
    | fields _time host sourcetype

The command limits the fields available in the resulting
events.

Important distinction:

    fields = control retained fields

## table

The table command creates a tabular result containing the
specified fields.

Example:

    index=_internal sourcetype=splunkd
    | table _time host sourcetype source

The result displays the selected fields as columns.

Important distinction:

    table = create a table from selected fields

## fields vs table

The two commands are related but serve different purposes.

    fields
        ->
    controls fields retained in the result

    table
        ->
    creates a tabular presentation of selected fields

This distinction was reinforced during the hands-on labs.

## Search Reasoning Model

When approaching a Search and Reporting problem:

    What events do I need?
        ->
    Choose the index

    What content or field value identifies them?
        ->
    Search term or field=value

    Do I need multiple conditions?
        ->
    AND / OR / NOT

    Is the field value patterned?
        ->
    Wildcard

    What time period should be searched?
        ->
    earliest / latest

    Which fields do I need?
        ->
    fields

    Do I need a tabular result?
        ->
    table

## Important Distinctions

    Basic search = search event content

    field=value = targeted field filtering

    OR = either condition

    AND = both conditions

    NOT = exclusion

    wildcard = pattern matching

    earliest/latest = time boundaries

    fields = control retained fields

    table = tabular presentation

## Module 06 Learning Outcome

The major improvement from this module was understanding how
different search conditions progressively narrow or control
the result set.

    Indexed Events
        ->
    Search
        ->
    Field Filtering
        ->
    Boolean Logic
        ->
    Wildcard Matching
        ->
    Time Filtering
        ->
    Field Selection
        ->
    Reporting

Module 06 theory, brainstorming, question practice, and
hands-on laboratory work are complete.

