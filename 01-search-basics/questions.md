# Splunk Core - 01 Search Basics Questions

## Q1 - Index

Question:

What is the purpose of specifying the index in a Splunk search?

Search:

index=_internal

Answer:

The index specifies where Splunk should search for indexed events.

Score:

10/10

---

## Q2 - Search Command

Question:

Why can this search work without explicitly writing the search command?

Search:

index=_internal error

Answer:

Splunk can use the search command implicitly at the beginning of the search.

The index is `_internal` and `error` is the search term.

Score:

8/10

Learning correction:

The question asked why the search command can be omitted, not only what
the search searches for.

---

## Q3 - Phrase Search

Question:

What is the purpose of quotation marks here?

Search:

index=_internal "connection refused"

Answer:

The quotation marks make `connection refused` a phrase search.

The search looks for the phrase as a combined expression rather than
treating the terms as separate words.

Score:

10/10

---

## Q4 - Field Search

Question:

What does this search look for?

Search:

index=_internal host=web01

Answer:

It searches the `_internal` index for events where the `host` field
matches `web01`.

Score:

10/10

---

## Q5 - Field Existence

Question:

What does this search test?

Search:

index=_internal status=*

Answer:

It searches for events in `_internal` where the `status` field exists
and has a value.

Score:

9/10

Learning correction:

`status=*` does not mean "list every status value." It filters for events
where the status field is present.

---

## Q6 - Boolean OR

Question:

What does this search return?

Search:

index=_internal error OR warning

Answer:

It searches the `_internal` index for events containing `error` or
`warning`.

OR broadens the search to accept either condition.

Score:

10/10

---

## Q7 - Time Range

Question:

What time period does this search request?

Search:

index=_internal earliest=-1h latest=now

Answer:

It searches the `_internal` index from one hour ago through the current
time.

`earliest=-1h` defines the beginning of the time range.

`latest=now` defines the end of the time range.

Score:

10/10

---

## Q8 - Pipe

Question:

What does the pipe do here?

Search:

index=_internal
| search error

Answer:

The pipe passes the results from the first part of the search to the
next command.

The first search finds events in `_internal`.

The second search filters those results for `error`.

Score:

10/10

---

## Q9 - Search Refinement

Question:

Explain the difference between:

index=_internal

and:

index=_internal error host=web01 earliest=-1h

Answer:

The first search is broad and searches the `_internal` index.

The second search narrows the results by:

- error condition
- host=web01
- last hour

The second search is more specific because additional evidence and
conditions reduce the result set.

Score:

10/10

---

## Q10 - No Results

Question:

A search that previously returned events suddenly returns zero results.

What should you investigate before adding more search conditions?

Answer:

Use an evidence-based troubleshooting sequence:

No results
  ->
Verify index
  ->
Verify time range
  ->
Verify search term or phrase
  ->
Verify field name
  ->
Verify field value
  ->
Verify data availability
  ->
Then refine the SPL

Do not assume that `error` is a field named `status`, or that a field
value is represented exactly as expected.

Score:

7/10

Learning correction:

The answer identified time, host, and status as possible things to
investigate, but the stronger approach is to validate one condition at a
time instead of changing multiple assumptions at once.

---

# Session Results

Questions completed:

10 / 10

Session score:

94%

Calculation:

94 / 100

Strong areas:

- Index reasoning
- Phrase searches
- Field-value searches
- Boolean OR
- Time ranges
- Pipes
- Search refinement
- stats reasoning

Weak areas:

- Reading the exact question requirement
- Distinguishing a field from an index
- Field existence versus enumerating values
- Evidence-based troubleshooting sequence

Primary learning lesson:

Understand the data operation first, then construct the SPL.

SPL syntax is the tool used to express the reasoning.
