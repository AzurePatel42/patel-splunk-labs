# Module 05 — Stats and Visualization Questions

## Question Practice Status

Module 05 theory and brainstorming were completed before the
hands-on laboratory work.

Approximately 45 questions were completed.

The question practice focused on using statistical functions,
grouping results, filtering aggregated results, sorting and
limiting result sets, and selecting appropriate visualization
approaches.

## Commands Tested

- stats
- eval
- where
- sort
- head
- count
- dc
- min
- max
- avg
- count(eval())

## Key Question Concepts

### stats

Used when the task is to aggregate events and produce statistical
results.

Example:

    | stats count by sourcetype

Important distinction:

    stats = aggregate and reduce

### eval

Used when the task is to create or calculate a field.

Example:

    | eval kb_per_event=len(_raw)/1024

Important distinction:

    eval = calculate or create a field

### where

Used when the task is to filter results based on a condition.

Example:

    | where count > 1000

Important distinction:

    where = filter

### sort

Used to order results.

Example:

    | sort - count

Important distinction:

    - field = descending
    + field = ascending

### head

Used to limit the number of results.

Example:

    | head 10

Important distinction:

    head = limit result set

### dc()

Used to calculate the distinct count of a field.

Example:

    | stats dc(host) as unique_hosts by sourcetype

Important distinction:

    dc() = distinct count

### min() and max()

Used to identify the minimum and maximum values.

Example:

    | stats min(_time) as earliest max(_time) as latest by sourcetype

Important distinction:

    min() = lowest value
    max() = highest value

### avg()

Used to calculate an average.

Example:

    | stats avg(kb_per_event) as avg_kb_per_event by sourcetype

Important distinction:

    avg() = average value

### count(eval())

Used to perform conditional counting inside stats.

Example:

    | stats count as total_events
        count(eval(sourcetype="splunkd")) as splunkd_events

Important distinction:

    count(eval()) = conditional count

## Key Command Selection Model

When deciding which command or function to use:

    Need to aggregate events?
        -> stats

    Need to create or calculate a field?
        -> eval

    Need to filter aggregated results?
        -> where

    Need to order results?
        -> sort

    Need to limit the result set?
        -> head

    Need a distinct count?
        -> dc()

    Need the lowest value?
        -> min()

    Need the highest value?
        -> max()

    Need an average?
        -> avg()

    Need conditional counting?
        -> count(eval())

## Important Corrections From Practice

A major command-selection distinction was reinforced:

    stats -> aggregation
    eval  -> field calculation
    where -> filtering
    sort  -> ordering
    head  -> limiting

Another important distinction was understanding the order of
operations when creating Top-N reports:

    | stats count by sourcetype
    | sort - count
    | head 10

The aggregation must happen before the result set can be
sorted and limited.

## Question Practice Completion

- Theory: COMPLETE
- Brainstorming: COMPLETE
- Approximately 45 questions: COMPLETE
- Hands-on lab validation: COMPLETE
- Formal question review: COMPLETE

The questions do not need to be repeated before moving forward.
The remaining Module 05 work is documentation cleanup and Git.
