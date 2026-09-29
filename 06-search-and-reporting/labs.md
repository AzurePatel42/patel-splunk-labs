# Module 06 — Search and Reporting Labs

## Lab 01 — Basic Search and Event Filtering

### Objective
Use a basic search to find events containing the term error.

### SPL
index=_internal error

### Result
- Events: 5,039
- Time range: Last 24 hours

### Key Learning
A basic search term filters raw events based on matching event content.


## Lab 02 — Multiple Conditions

### Objective
Search for events containing either error or warn.

### SPL
index=_internal error OR warn

### Result
- Events: 5,485
- Time range: Last 24 hours

### Key Learning
The OR operator allows a search to match either of multiple conditions.


## Lab 03 — Field Search

### Objective
Search for error events from the splunkd sourcetype.

### SPL
index=_internal sourcetype=splunkd error

### Result
- Events: 37
- Time range: Last 24 hours

### Key Learning
Field-value searches narrow the search by requiring a specific field to contain a specified value.


## Lab 04 — Exact Sourcetype Filtering

### Objective
Return events belonging to the splunkd_access sourcetype.

### SPL
index=_internal sourcetype=splunkd_access

### Result
- Events: 21,255
- Time range: Last 24 hours

### Key Learning
Specifying a field-value pair provides targeted filtering of indexed events.


## Lab 05 — Time-Based Search

### Objective
Search for splunkd events occurring during the last one hour.

### SPL
index=_internal sourcetype=splunkd earliest=-1h latest=now

### Result
- Events: 16,292
- Time range: Last 1 hour

### Key Learning
Search time modifiers can restrict the search to a specific time window.


## Lab 06 — Multiple Conditions with AND

### Objective
Find splunkd events containing both error and warn.

### SPL
index=_internal sourcetype=splunkd error AND warn

### Result
- Events: 33
- Time range: Last 24 hours

### Key Learning
The AND operator requires both search conditions to match.


## Lab 07 — Wildcard Search

### Objective
Search for events from sourcetypes beginning with 
ode:sidecar:.

### SPL
index=_internal sourcetype=node:sidecar:*

### Result
- Events: 35,044
- Time range: Last 24 hours

### Key Learning
Wildcards allow searches to match multiple field values that share a common pattern.


## Lab 08 — Exclusion with NOT

### Objective
Find splunkd events that do not contain the term error.

### SPL
index=_internal sourcetype=splunkd NOT error

### Result
- Events: 399,329
- Time range: Last 24 hours

### Key Learning
The NOT operator excludes events matching the specified search condition.


## Lab 09 — Field Selection with fields

### Objective
Restrict the fields returned by the search to the selected fields.

### SPL
index=_internal sourcetype=splunkd
| fields _time host sourcetype

### Result
- Events: 399,490
- Time range: Last 24 hours

### Key Learning
The fields command controls which fields are retained in the search results.


## Lab 10 — Field Selection with table

### Objective
Create a tabular result containing selected fields.

### SPL
index=_internal sourcetype=splunkd
| table _time host sourcetype source

### Result
- Events: 399,705
- Time range: Last 24 hours

### Key Learning
The table command creates a tabular result containing the specified fields.


# Module 06 Summary

| Lab | Topic | Status |
|---:|---|:---:|
| 01 | Basic search and event filtering | Complete |
| 02 | Multiple conditions with OR | Complete |
| 03 | Field search | Complete |
| 04 | Exact sourcetype filtering | Complete |
| 05 | Time-based search | Complete |
| 06 | Multiple conditions with AND | Complete |
| 07 | Wildcard search | Complete |
| 08 | Exclusion with NOT | Complete |
| 09 | Field selection with fields | Complete |
| 10 | Field selection with table | Complete |

## Final Status

Module 06 — Search and Reporting: 10/10 Labs Complete
