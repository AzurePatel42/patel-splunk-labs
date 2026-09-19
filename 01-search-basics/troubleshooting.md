# Splunk Core - 01 Search Basics Troubleshooting

## Core Troubleshooting Model

No Results
  ->
Verify index
  ->
Verify time range
  ->
Verify search term
  ->
Verify field name
  ->
Verify field value
  ->
Verify data availability
  ->
Refine SPL

---

# Problem 1 - Wrong Index

Symptom:

A search returns zero events.

Reasoning:

First verify that the selected index contains the expected data.

Do not assume that a host, sourcetype, or other value is an index.

Example distinction:

web
  ->
possible index

web01
  ->
possible host

---

# Problem 2 - Time Range Too Narrow

Symptom:

The SPL appears correct but no expected events are returned.

Check:

earliest

latest

Also check the time range selected in the Splunk interface.

A correct search with an incorrect time range can still return zero
results.

---

# Problem 3 - Search Term Does Not Match the Data

Symptom:

index=web error

returns nothing.

Check:

Does the raw event actually contain the word `error`?

Could the event use another representation?

Examples:

status=500

level=error

message="..."

Use evidence from real events rather than assuming the field structure.

---

# Problem 4 - Wrong Field Name

Symptom:

A field-value search returns no expected results.

Check:

- Exact field name
- Field spelling
- Whether the field exists in the selected events

Do not assume that an English requirement such as "authentication
failure" corresponds to a field named `status`.

---

# Problem 5 - Wrong Field Value

Symptom:

The field exists but the field-value search returns no expected events.

Check the actual values present in the events.

Example:

status="authentication failure"

must match how the data actually represents that condition.

---

# Problem 6 - Too Many Conditions

Symptom:

A broad search returns events, but a refined search returns none.

Method:

Remove the newest condition.

Run the search again.

Add conditions one at a time.

This identifies which condition changed the result set.

---

# Problem 7 - Overly Broad Search

Symptom:

The search returns a very large result set.

Improve the search using evidence:

Index
  ->
Time
  ->
Keyword / phrase
  ->
Field
  ->
Additional processing

Avoid adding arbitrary conditions.

---

# Problem 8 - Aggregation Confusion

Symptom:

The administrator expects original events but receives summary rows.

Reason:

stats transforms the current event set into aggregated results.

Example:

| stats count by host

returns summary information per host.

If original events are needed, a different retrieval approach is required.

---

# Problem 9 - Head versus Tail Confusion

Symptom:

The selected results are the opposite of the intended result.

Check the sort direction.

Descending:

| sort - avg_response_time
| head 5

Ascending:

| sort avg_response_time
| tail 5

The selection command depends on result ordering.

---

# Problem 10 - Evidence-Based Troubleshooting

Do not change multiple assumptions at once.

Use:

Evidence
  ->
One hypothesis
  ->
One change
  ->
Observe
  ->
Next hypothesis

Primary principle:

Do not guess the data model.

Inspect the evidence and then construct or refine the search.
