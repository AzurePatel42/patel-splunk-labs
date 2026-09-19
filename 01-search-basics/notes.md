# Splunk Core - 01 Search Basics

## Module Objective

Learn how to retrieve the right events from Splunk indexes using basic
Search Processing Language (SPL).

Core focus:

- Index scope
- Keywords
- Phrases
- Field-value searches
- Boolean logic
- Wildcards
- Time ranges
- Search pipeline
- Evidence-based search refinement

---

# 1. Index

An index is the data location being searched.

Example:

index=_internal

Mental model:

Index
  ->
Search indexed events
  ->
Results

---

# 2. Search

A search retrieves events that match search criteria.

At the beginning of a search, the search command is implied.

These are equivalent:

host=web01

search host=web01

The search command can use:

- Keywords
- Phrases
- Field-value pairs
- Boolean expressions
- Comparison expressions
- Time expressions
- Index expressions

---

# 3. Keyword Search

A keyword searches event data.

Example:

index=_internal error

This searches the _internal index for events containing error.

Keyword searches are not case sensitive.

---

# 4. Phrase Search

Use double quotation marks when the words must appear together as a phrase.

Example:

index=_internal "connection refused"

Without quotation marks, multiple terms can behave as separate search terms.

---

# 5. Field-Value Search

Use:

field=value

Example:

index=_internal host=web01

This searches for events where the host field matches web01.

Field names are case sensitive.
Field values are not case sensitive.

---

# 6. Field Existence

Use a wildcard to test whether a field exists.

Example:

index=_internal status=*

This returns events containing the status field.

---

# 7. Boolean Logic

Use Boolean operators to combine search conditions.

AND:

index=_internal error AND warning

OR:

index=_internal error OR warning

NOT:

index=_internal NOT debug

Important:

Multiple search terms without an explicit OR are generally treated as
an AND-style requirement.

Use parentheses when grouping more complex conditions.

Example:

index=_internal (error OR warning)

---

# 8. Wildcards

The asterisk * can match varying characters.

Example:

index=_internal host=web*

Wildcards can be useful for related host names.

Avoid unnecessarily broad wildcard searches because broad searches can
consume more system resources.

Be as specific as practical.

---

# 9. Time Range

A search can be restricted by time.

Relative example:

index=_internal earliest=-1h latest=now

This searches the previous hour.

Common relative patterns:

- -15m = last 15 minutes
- -1h = last hour
- -24h = last 24 hours
- -7d = last 7 days

---

# 10. Search Pipeline

The pipe passes the current search results to the next command.

Example:

index=_internal
| search error

Mental model:

Search
  ->
Results
  ->
Next command
  ->
New results

The first search reduces the event set.
Later commands process the current result set.

---

# 11. Search Refinement

Use evidence to make a search more specific.

Example progression:

index=_internal

index=_internal error

index=_internal error host=web01

index=_internal error host=web01 earliest=-1h

Mental model:

Broad
  ->
Filter
  ->
Refine
  ->
Analyze

---

# 12. Core Search Mental Model

Question:

What data am I searching?

  ->
Which index?

  ->
What events do I need?

  ->
Keyword / phrase / field

  ->
What time range?

  ->
Earliest / latest

  ->
Do I need additional processing?

  ->
Pipe

---

# Brainstorming Record - Search Basics Q1-Q10

## Q1 - Authentication Failures

Requirement:

Find authentication failures from web servers during the last hour and
count the matching events.

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

Initial reasoning:

- Index identifies where the data is searched.
- Host or another appropriate field identifies the web servers.
- Authentication failure must be represented by the correct event field/value.
- earliest and latest restrict the time window.
- stats count summarizes the matching events.

---

## Q2 - Narrowing a Large Result Set

Starting search:

index=web "authentication failure"

Requirement:

Only web01 during the last hour.

Correction:

Do not change the index from web to web01.

web = index

web01 = host

Reasoning:

Keep the index:

index=web

Add:

host=web01

Add:

earliest=-1h latest=now

Mental model:

Correct index
  ->
Correct host
  ->
Correct time range
  ->
Smaller evidence set

---

## Q3 - Count Authentication Failures

Requirement:

Find how many authentication failures occurred on web01 during the
last hour.

Correct pattern:

index=web host=web01 status="authentication failure" earliest=-1h latest=now
| stats count

Reasoning:

Filter first.

Then count the remaining events.

Mental model:

Search
  ->
Filter
  ->
Time restriction
  ->
Aggregate
  ->
Count

---

## Q4 - Count Failures by Web Server

Requirement:

Find how many authentication failures each web server had during the
last hour.

Correct aggregation:

| stats count by host

Reasoning:

stats count

  ->
Counts events

by host

  ->
Groups the count for each host

Mental model:

Events
  ->
Group by host
  ->
Count each group

---

## Q5 - Average Response Time by Host

Requirement:

Show the average response time for each web server during the last hour.

Correct pattern:

index=web host=web* earliest=-1h latest=now
| stats avg(response_time) by host

Reasoning:

- Select the web data.
- Identify web-server hosts.
- Restrict to the last hour.
- Calculate average response time.
- Group the average by host.

Mental model:

Filter
  ->
Average
  ->
Group by host

---

## Q6 - Filter Slow Servers

Requirement:

Show only servers whose average response time is greater than 500.

Correct pattern:

index=web host=web* earliest=-1h latest=now
| stats avg(response_time) as avg_response_time by host
| where avg_response_time > 500

Reasoning:

stats creates one average value per host.

where then evaluates the calculated result.

Important lesson:

stats
  ->
Calculate / aggregate

where
  ->
Filter the calculated results

---

## Q7 - Top Five Slowest Servers

Requirement:

Show the five web servers with the highest average response time.

Correct pattern:

index=web host=web* earliest=-1h latest=now
| stats avg(response_time) as avg_response_time by host
| sort - avg_response_time
| head 5

Reasoning:

- Calculate average response time by host.
- Sort highest to lowest.
- Keep the first five.

Alternative reasoning:

sort avg_response_time
  ->
Lowest to highest

tail 5
  ->
Highest five

Important lesson:

head or tail depends on sort direction.

---

## Q8 - Slow Servers Above a Threshold

Requirement:

Show only servers whose average response time is greater than 500,
then show the five highest averages.

Correct pattern:

index=web host=web* earliest=-1h latest=now
| stats avg(response_time) as avg_response_time by host
| where avg_response_time > 500
| sort - avg_response_time
| head 5

Mental model:

Raw events
  ->
Filter scope
  ->
Average by host
  ->
Filter average
  ->
Sort
  ->
Top five

Important lesson:

Filter the aggregated result before selecting the final top five.

---

## Q9 - Original Events from the Slowest Hosts

Requirement:

Find the five slowest hosts by average response time and then return
all original events from those hosts.

Important distinction:

stats summarizes events.

The original events are no longer represented as individual rows after
the aggregation.

Reasoning:

Stage 1:
Find the five hosts.

Stage 2:
Use those host values to retrieve the original events.

A subsearch can be used to pass the host values from stage 1 into the
outer search.

Example pattern:

index=web host=web* earliest=-1h latest=now
[
    search index=web host=web* earliest=-1h latest=now
    | stats avg(response_time) as avg_response_time by host
    | sort - avg_response_time
    | head 5
    | fields host
]

Important lesson:

Aggregation and original-event retrieval are different operations.

---

## Q10 - Zero Results

Requirement:

A search that previously returned events now returns zero results.

Do not immediately add more search conditions.

Troubleshooting sequence:

No results
  ->
Verify index
  ->
Verify time range
  ->
Verify search term / phrase
  ->
Verify field name
  ->
Verify field value
  ->
Verify data availability
  ->
Then refine the SPL

Important lesson:

Use evidence before changing the search.

Do not assume:

- The index is wrong.
- A keyword is a field.
- A field is named exactly as expected.
- A field value is represented exactly as expected.

---

# Search Basics Brainstorming Summary

Core reasoning:

What data am I searching?
  ->
Which index?
  ->
Which events?
  ->
Which field / keyword / phrase?
  ->
What time range?
  ->
Do I need aggregation?
  ->
Do I need to filter the aggregated result?
  ->
Do I need sorting or limiting?
  ->
Do I need the original events instead of summary results?

Key principle:

Understand the data operation first, then construct the SPL.

SPL commands are tools for expressing the reasoning.
