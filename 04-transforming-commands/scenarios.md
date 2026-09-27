# Module 04 — Transforming Commands Scenarios

## Scenario 1 — Create a Calculated Field

### Situation

A web application stores response time in milliseconds, but the
operations team wants to analyze the values in seconds.

### Solution

Use `eval`:

    | eval response_seconds=response_time/1000

### Reasoning

`eval` creates the calculated `response_seconds` field without
removing the original event.

---

## Scenario 2 — Filter Slow and Failed Requests

### Situation

Find events where the HTTP status is 500 or greater and the response
time is greater than 5 seconds.

### Solution

Use `where`:

    | where status>=500 AND response_time>5

### Reasoning

`where` filters results based on a Boolean condition.

---

## Scenario 3 — Count Requests by Server

### Situation

Operations wants the number of requests handled by each web server.

### Solution

Use `stats`:

    | stats count as total_requests by server_host

### Result

    web01 = 4
    web02 = 4
    web03 = 4

---

## Scenario 4 — Calculate a Global Average Without Losing Events

### Situation

Calculate the overall average response time while keeping every
original event available for further analysis.

### Solution

Use `eventstats`:

    | eval response_seconds=response_time/1000
    | eventstats avg(response_seconds) as overall_avg

### Result

    12 events remained.

The `overall_avg` field was added to the events.

    overall_avg = 4.591666666666668

### Key Decision

Use `eventstats` rather than `stats` when the original events need
to remain available.

---

## Scenario 5 — Find the Slowest Server Group

### Situation

Operations wants to identify servers whose average response time
is greater than 4.5 seconds.

### Solution

    | eval response_seconds=response_time/1000
    | stats avg(response_seconds) as avg_response by server_host
    | where avg_response > 4.5
    | sort - avg_response

### Result

    web02 = 4.975 seconds

---

## Scenario 6 — Find the Most Frequently Used Endpoints

### Situation

Determine which API endpoints receive the most requests.

### Solution

Use `top`:

    | top limit=5 endpoint

### Result

    /api/orders     7
    /api/products   3
    /api/users      2

---

## Scenario 7 — Keep One Event Per Server

### Situation

The dataset contains multiple events from each server, but an
analyst needs only one event per server.

### Solution

Use `dedup`:

    | dedup server_host

### Result

    12 events -> 3 events

Remaining servers:

    web01
    web02
    web03

---

## Scenario 8 — Present Only Required Fields

### Situation

The analyst wants the final output to contain only server name,
endpoint, and response time.

### Solution

Use `table`:

    | table server_host endpoint response_time

---

## Scenario 9 — Sort Aggregated Results

### Situation

After counting requests by server, the analyst wants the highest
request count first.

### Solution

Use:

    | stats count as total_requests by server_host
    | sort - total_requests

### Key Decision

The minus sign indicates descending order.

---

## Scenario 10 — Build a Complete Analysis Pipeline

### Situation

An operations analyst wants to:

1. Extract the server name.
2. Convert response time to seconds.
3. Calculate average response time per server.
4. Keep only servers above 4.5 seconds.
5. Sort the results from slowest to fastest.
6. Display only the required fields.

### Solution

    index=main
    | rex "^(?<server_host>web\d+),"
    | eval response_seconds=response_time/1000
    | stats avg(response_seconds) as avg_response by server_host
    | where avg_response > 4.5
    | sort - avg_response
    | table server_host avg_response

### Result

    web02 = 4.975 seconds

## Scenario Completion

- Calculated fields: COMPLETE
- Event filtering: COMPLETE
- Aggregation: COMPLETE
- Event-preserving statistics: COMPLETE
- Deduplication: COMPLETE
- Sorting: COMPLETE
- Top values: COMPLETE
- Result presentation: COMPLETE
- Combined pipeline: COMPLETE
