# Module 02 - Weak Areas

## Primary Weak Area

Pipeline State and Result-Set Reasoning

The main refinement area identified during Module 02 was distinguishing raw events from statistical result rows and tracking how the result set changes as SPL commands execute.

Key points:

- `search` filters the current event set.
- `stats` transforms events into statistical result rows.
- Fields created by `stats` become available to later commands.
- `eval` can create or modify fields on the current result set.
- `where` evaluates conditions against the current result set.
- A later transforming command such as `stats` can create a new result set and change the meaning of fields such as `count`.
- Fields not included in a transforming command may not be preserved in the resulting output.

## Core Mental Model

Raw Events
->
search
->
Filtered Events
->
stats
->
Statistical Result Rows
->
eval / where / sort
->
Transformed Result Set
->
Another Transforming Command
->
New Result Set

## Critical Question

At every stage of an SPL pipeline, ask:

"What does the current result set contain right now?"

## Evidence

Formal Questions:
96%

Scenarios:
92%

Troubleshooting:
96%

The main weakness appeared in Formal Question 10 and the early scenario/troubleshooting questions, then improved through repeated result-set reasoning.

## Status

WEAK AREA IDENTIFIED

Next:
Retest
