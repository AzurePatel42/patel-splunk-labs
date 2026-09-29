# Module 06 — Search and Reporting Questions

## Question Practice Status

Module 06 theory and brainstorming were completed before the
hands-on laboratory work.

The question practice focused on selecting the correct search
syntax, Boolean operator, field filter, wildcard, time modifier,
and reporting command for a given requirement.

## Search Concepts Tested

- Basic search
- Field-value searches
- AND
- OR
- NOT
- Wildcards
- earliest
- latest
- fields
- table

## Key Question Concepts

### Basic Search

Used when the requirement is to find events containing a
specific term.

Example:

    index=_internal error

Important distinction:

    Search term = match event content

### Field-Value Search

Used when the requirement specifies a field and its value.

Example:

    index=_internal sourcetype=splunkd error

Important distinction:

    field=value = targeted filtering

### OR

Used when either condition can satisfy the search.

Example:

    index=_internal error OR warn

Important distinction:

    OR = either condition

### AND

Used when both conditions must be satisfied.

Example:

    index=_internal sourcetype=splunkd error AND warn

Important distinction:

    AND = both conditions

### NOT

Used when matching events must be excluded.

Example:

    index=_internal sourcetype=splunkd NOT error

Important distinction:

    NOT = exclusion

### Wildcards

Used when the exact field value is not known but follows a
recognizable pattern.

Example:

    index=_internal sourcetype=node:sidecar:*

Important distinction:

    * = pattern matching

### earliest / latest

Used to define the time boundaries of a search.

Example:

    index=_internal sourcetype=splunkd earliest=-1h latest=now

Important distinction:

    earliest = beginning of time range

    latest = end of time range

### fields

Used to control which fields are retained.

Example:

    | fields _time host sourcetype

Important distinction:

    fields = control retained fields

### table

Used to create a tabular result containing selected fields.

Example:

    | table _time host sourcetype source

Important distinction:

    table = tabular presentation

## Key Search Selection Model

When deciding how to construct a search:

    Need to find matching event content?
        -> search term

    Need a specific field value?
        -> field=value

    Need either condition?
        -> OR

    Need both conditions?
        -> AND

    Need to exclude a condition?
        -> NOT

    Need to match a field pattern?
        -> wildcard

    Need a specific time window?
        -> earliest / latest

    Need to control retained fields?
        -> fields

    Need a tabular result?
        -> table

## Important Distinctions From Practice

A major search-selection distinction was reinforced:

    Basic search
        = event content matching

    field=value
        = targeted field filtering

    AND
        = both conditions

    OR
        = either condition

    NOT
        = exclusion

    wildcard
        = pattern matching

    earliest/latest
        = time boundaries

    fields
        = retained field control

    table
        = tabular presentation

## Question Practice Completion

- Theory: COMPLETE
- Brainstorming: COMPLETE
- Question practice: COMPLETE
- Hands-on lab validation: COMPLETE
- Formal question review: COMPLETE

The questions do not need to be repeated before moving forward.
The remaining Module 06 work is scenario documentation and Git.
