# Module 04 — Transforming Commands

## Overview

Module 04 focuses on transforming, calculating, aggregating, filtering,
sorting, and presenting Splunk search results.

## Commands Covered

- eval
- where
- stats
- eventstats
- dedup
- sort
- top
- table

## Command Distinctions

    eval       -> create or modify fields
    where      -> filter events/results
    stats      -> aggregate and reduce results
    eventstats -> calculate statistics while keeping events
    dedup      -> remove duplicate events
    sort       -> order results
    top        -> find common values
    table      -> select displayed fields

## Lab Results

### eval

    | eval response_seconds=response_time/1000

Converts response time from milliseconds to seconds.

### stats

    | stats count as total_requests by server_host

Results:

    web01 = 4
    web02 = 4
    web03 = 4

Total events: 12.

### eventstats

    | eventstats avg(response_seconds) as overall_avg

Results:

    12 events remained.
    overall_avg = 4.591666666666668

Key distinction:

    stats      -> aggregates and reduces the result set
    eventstats -> calculates statistics while keeping events

### dedup

    | dedup server_host

Result:

    12 events -> 3 events

One event remained for each server:

    web01
    web02
    web03

### sort

    | sort - total_requests

The minus sign performs descending sorting.

### top

    | top limit=5 endpoint

Results:

    /api/orders     7
    /api/products   3
    /api/users      2

### table

    | table server_host endpoint response_time

Selects the fields displayed in the final results.

## Complete Transforming Pipeline

    transform
        ->
    aggregate
        ->
    filter
        ->
    sort
        ->
    select

Example:

    | eval ...
    | stats ...
    | where ...
    | sort ...
    | table ...

## Hands-On Validation

### Response Time by Server

    web01 = 4.45 seconds
    web02 = 4.975 seconds
    web03 = 4.35 seconds

### Servers Above 4.5 Seconds

    web02 = 4.975 seconds

### Endpoint Frequency

    /api/orders     7
    /api/products   3
    /api/users      2

### Overall Average

    overall_avg = 4.591666666666668

## Module Progress

- Theory: COMPLETE
- Brainstorming/questions: COMPLETE
- Approximately 45 questions: COMPLETE
- Hands-on labs: COMPLETE
- Documentation: COMPLETE
- Git commit/push: PENDING

## Key Learning

Module 04 reinforced the difference between transforming,
aggregating, filtering, preserving events, deduplicating,
sorting, and presenting Splunk results.

The most important distinction is:

    stats
        -> reduces events into aggregated results

    eventstats
        -> calculates statistics while preserving events

Module 04 laboratory validation is COMPLETE.
