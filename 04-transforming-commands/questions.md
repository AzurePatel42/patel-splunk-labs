# Module 04 — Transforming Commands Questions

## Question Practice Status

Module 04 theory and brainstorming were completed before the
hands-on laboratory work.

Approximately 45 questions were completed.

The question practice focused on selecting the correct transforming
command and understanding how commands change the result set.

## Commands Tested

- eval
- where
- stats
- eventstats
- dedup
- sort
- top
- table

## Key Question Concepts

### eval

Used when the task is to create or modify a field.

Example:

    | eval slow_request=if(response_time>5,"yes","no")

### where

Used when the task is to filter results based on a condition.

Example:

    | where status>=500 AND response_time>5

Important distinction:

    where = filter

### stats

Used to aggregate events.

Example:

    | stats count as total_requests by server_host

Important distinction:

    stats = aggregate and reduce

### eventstats

Used to calculate statistics while keeping the original events.

Example:

    | eventstats avg(response_seconds) as overall_avg

Important distinction:

    eventstats = calculate and keep events

### dedup

Used to remove duplicate events based on a field.

Example:

    | dedup server_host

Result:

    Multiple events per server
        ->
    One event per server

### sort

Used to order results.

Example:

    | sort - total_requests

Important distinction:

    - field = descending
    + field = ascending

### top

Used to identify the most common values.

Example:

    | top limit=5 endpoint

### table

Used to select the fields displayed in the final result.

Example:

    | table server_host endpoint response_time

## Key Command Selection Model

When deciding which command to use:

    Need to create/change a field?
        -> eval

    Need to filter results?
        -> where

    Need to aggregate/reduce results?
        -> stats

    Need statistics while keeping events?
        -> eventstats

    Need to remove duplicates?
        -> dedup

    Need to order results?
        -> sort

    Need most common values?
        -> top

    Need specific displayed fields?
        -> table

## Important Corrections From Practice

A major command-selection distinction was reinforced:

    where -> filtering
    eval  -> field creation/modification
    stats -> aggregation

For example, to find events where both conditions are true:

    | where status>=500 AND response_time>5

To create a classification field:

    | eval slow_request=if(response_time>5,"yes","no")

To aggregate:

    | stats count by server_host

## Question Practice Completion

- Theory: COMPLETE
- Brainstorming: COMPLETE
- Approximately 45 questions: COMPLETE
- Hands-on lab validation: COMPLETE
- Formal question review: COMPLETE

The questions do not need to be repeated before moving forward.
The remaining Module 04 work is documentation cleanup and Git.
