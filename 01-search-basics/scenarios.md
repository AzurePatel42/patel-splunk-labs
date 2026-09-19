# Splunk Core - 01 Search Basics Scenarios

## Scenario 1 - Authentication Failures

Requirement:

Find authentication failures from web servers during the last hour
and count the matching events.

Reasoning:

Index
  ->
Identify web-server events
  ->
Identify authentication failures
  ->
Restrict to last hour
  ->
Count matching events

Example pattern:

index=web host=web01 status="authentication failure" earliest=-1h latest=now
| stats count

Important:

The correct field and value must be verified from the data.

---

## Scenario 2 - Count Failures by Host

Requirement:

Find how many authentication failures each web server had during the
last hour.

Example pattern:

index=web host=web* earliest=-1h latest=now
| stats count by host

Reasoning:

stats count
  ->
count events

by host
  ->
group the count for each host

---

## Scenario 3 - Average Response Time

Requirement:

Show the average response time for each web server during the last
hour.

Example pattern:

index=web host=web* earliest=-1h latest=now
| stats avg(response_time) by host

Reasoning:

Filter
  ->
Average
  ->
Group by host

---

## Scenario 4 - Slow Servers

Requirement:

Show only servers whose average response time is greater than 500 and
return the five highest averages.

Example pattern:

index=web host=web* earliest=-1h latest=now
| stats avg(response_time) as avg_response_time by host
| where avg_response_time > 500
| sort - avg_response_time
| head 5

Reasoning:

Raw events
  ->
Aggregate by host
  ->
Filter calculated average
  ->
Sort highest first
  ->
Keep five

---

## Scenario 5 - Head versus Tail

Requirement:

Return the five highest average response times.

Two valid selection patterns can express the same result depending on
sort direction.

Descending:

| sort - avg_response_time
| head 5

Ascending:

| sort avg_response_time
| tail 5

Key lesson:

Selection depends on the ordering of the results.

---

## Scenario 6 - Original Events from Selected Hosts

Requirement:

Find the five slowest hosts by average response time and then return
their original events.

Reasoning:

Stage 1:
Calculate the average by host.

Stage 2:
Select the five hosts.

Stage 3:
Use those host values to retrieve the original events.

Important distinction:

stats summarizes events.

Original-event retrieval is a separate operation.

A subsearch can pass selected host values into the outer search.

Example pattern:

index=web host=web* earliest=-1h latest=now
[
    search index=web host=web* earliest=-1h latest=now
    | stats avg(response_time) as avg_response_time by host
    | sort - avg_response_time
    | head 5
    | fields host
]

---

## Scenario 7 - Zero Results

Requirement:

A search that previously returned events now returns zero results.

Reasoning:

Do not immediately add host, status, or other filters.

Validate:

Index
  ->
Time range
  ->
Search term
  ->
Field name
  ->
Field value
  ->
Data availability

Then make one change and observe the result.

---

# Scenario Reasoning Principle

Start with the requirement.

Translate the requirement into data operations.

Then translate those operations into SPL.

Requirement
  ->
Data operation
  ->
SPL
  ->
Results
  ->
Interpretation
