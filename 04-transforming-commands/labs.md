# Module 04 — Transforming Commands Labs

## Lab Environment

Splunk Enterprise 10.4.3

Test data was uploaded into the `main` index.

The dataset contains 12 web request events.

The raw event data required extraction of the server name using:

    | rex "^(?<server_host>web\d+),"

This created the `server_host` field used throughout the labs.

---

## Lab 1 — eval

### Objective

Convert response time from milliseconds to seconds.

### SPL

    index=main
    | rex "^(?<server_host>web\d+),"
    | eval response_seconds=response_time/1000

### Result

The `response_seconds` field was successfully calculated from
`response_time`.

---

## Lab 2 — stats

### Objective

Count requests by server.

### SPL

    index=main
    | rex "^(?<server_host>web\d+),"
    | stats count as total_requests by server_host

### Result

    web01 = 4
    web02 = 4
    web03 = 4

Total events: 12.

---

## Lab 3 — stats + sort

### Objective

Count requests by server and sort from highest to lowest.

### SPL

    index=main
    | rex "^(?<server_host>web\d+),"
    | stats count as total_requests by server_host
    | sort - total_requests

### Result

    web01 = 4
    web02 = 4
    web03 = 4

The `-` operator sorted the results in descending order.

---

## Lab 4 — top

### Objective

Find the most common endpoints.

### SPL

    index=main
    | top limit=5 endpoint

### Result

    /api/orders     7
    /api/products   3
    /api/users      2

---

## Lab 5 — eval + stats

### Objective

Calculate average response time in seconds for each server.

### SPL

    index=main
    | rex "^(?<server_host>web\d+),"
    | eval response_seconds=response_time/1000
    | stats avg(response_seconds) as avg_seconds by server_host

### Result

    web01 = 4.45 seconds
    web02 = 4.975 seconds
    web03 = 4.35 seconds

---

## Lab 6 — stats + where

### Objective

Find servers whose average response time is greater than
4.5 seconds.

### SPL

    index=main
    | rex "^(?<server_host>web\d+),"
    | eval response_seconds=response_time/1000
    | stats avg(response_seconds) as avg_response by server_host
    | where avg_response > 4.5
    | sort - avg_response

### Result

    web02 = 4.975 seconds

Only `web02` exceeded the 4.5-second threshold.

---

## Lab 7 — eventstats

### Objective

Calculate the overall average while keeping the original events.

### SPL

    index=main
    | rex "^(?<server_host>web\d+),"
    | eval response_seconds=response_time/1000
    | eventstats avg(response_seconds) as overall_avg

### Result

    12 events remained.

The `overall_avg` field was added to the events.

    overall_avg = 4.591666666666668

Key distinction:

    stats      -> aggregates and reduces the result set
    eventstats -> calculates statistics while keeping events

---

## Lab 8 — dedup

### Objective

Keep one event for each server.

### SPL

    index=main
    | rex "^(?<server_host>web\d+),"
    | dedup server_host
    | table server_host endpoint response_time

### Result

    12 events -> 3 events

Servers remaining:

    web01
    web02
    web03

---

## Lab 9 — table

### Objective

Select specific fields for final presentation.

### SPL

    index=main
    | rex "^(?<server_host>web\d+),"
    | dedup server_host
    | table server_host endpoint response_time

### Result

The final results displayed:

    server_host
    endpoint
    response_time

---

## Lab 10 — Complete Transforming Pipeline

### Objective

Combine transformation, aggregation, filtering, sorting,
and presentation.

### SPL

    index=main
    | rex "^(?<server_host>web\d+),"
    | eval response_seconds=response_time/1000
    | stats avg(response_seconds) as avg_response by server_host
    | where avg_response > 4.5
    | sort - avg_response
    | table server_host avg_response

### Result

    web02 = 4.975 seconds

---

## Module 04 Lab Completion

- eval: COMPLETE
- stats: COMPLETE
- eventstats: COMPLETE
- where: COMPLETE
- sort: COMPLETE
- top: COMPLETE
- dedup: COMPLETE
- table: COMPLETE
- Combined pipeline: COMPLETE

Theory: COMPLETE

Brainstorming/questions: COMPLETE

Approximately 45 questions: COMPLETE

Hands-on labs: COMPLETE

Documentation: COMPLETE

Git commit/push: PENDING

Module 04 is ready for Git commit and push.
