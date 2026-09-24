## Lab 9 - stats Before eval Classification

### Objective

Use stats to create a count field before using eval and case() to classify statistical result rows by volume.

---

### SPL

index=_internal
| search error
| stats count by sourcetype
| eval volume=case(count>200,"HIGH",count>50,"MEDIUM",true(),"LOW")
| sort - count

---

### Execution Result

Search completed successfully.

Events:

950

Statistics rows:

10

The results contained the following classification fields:

sourcetype

count

volume

Results included:

node:sidecar:postgres:stdout

count:

280

volume:

HIGH

splunkd

count:

267

volume:

HIGH

splunkd_ui_access

count:

187

volume:

MEDIUM

node:supervisor

count:

131

volume:

MEDIUM

postgres

count:

50

volume:

LOW

---

### Result Interpretation

The search returned 950 events matching the search term:

error

The stats command grouped the events by:

sourcetype

For each sourcetype, stats created a count field.

The eval command then used the count field to classify each statistical result row.

The case() conditions were:

count > 200 -> HIGH

count > 50 -> MEDIUM

otherwise -> LOW

The node:sidecar:postgres:stdout group had count = 280.

Because 280 is greater than 200, its volume was classified as HIGH.

The splunkd group had count = 267 and was also classified as HIGH.

The splunkd_ui_access group had count = 187 and was classified as MEDIUM.

The node:supervisor group had count = 131 and was classified as MEDIUM.

The postgres group had count = 50 and was classified as LOW because the condition was count > 50, not count >= 50.

---

### Execution Flow

index=_internal
    ->
search error
    ->
950 matching events
    ->
stats count by sourcetype
    ->
10 statistical result rows
    ->
count field created
    ->
eval case() evaluates count
    ->
HIGH / MEDIUM / LOW classification
    ->
sort - count

---

### Key Learning

Command execution order matters in SPL.

stats must create the count field before eval can evaluate count.

eval operates on the statistical result rows produced by stats.

case() evaluates conditions in order.

The first true condition determines the classification.

The true() condition provides the fallback classification.

A threshold using > does not include the threshold value itself.

Therefore count=50 is LOW when the condition is count>50.

---

### Troubleshooting / Verification

The initial search placed eval before stats.

That produced LOW classifications because count had not yet been created by stats.

The corrected search placed stats before eval.

The corrected search successfully produced HIGH, MEDIUM, and LOW classifications.

No event sampling was reported.

The Statistics tab was selected.

The result contained 10 statistical rows.

---

### Lab Status

COMPLETE

Understanding:

10 / 10

---

## Module 02 Lab Progress

Lab 1:

COMPLETE

Lab 2:

COMPLETE

Lab 3:

COMPLETE

Lab 4:

COMPLETE

Lab 5:

COMPLETE

Lab 6:

COMPLETE

Lab 7:

COMPLETE

Lab 8:

COMPLETE

Lab 9:

COMPLETE

Next:

Lab 10
## Lab 10 - Combined stats eval Classification

### Objective

Combine stats aggregation with eval and case() classification to analyze event volume and average linecount by sourcetype.

---

### SPL

index=_internal
| search error
| stats count avg(linecount) by sourcetype
| eval volume=case(count>200,"HIGH",count>50,"MEDIUM",true(),"LOW")
| sort - count

---

### Execution Result

Search completed successfully.

Events:

1014

Statistics rows:

10

The results contained the following fields:

sourcetype

count

avg(linecount)

volume

Results included:

node:sidecar:postgres:stdout

count:

295

avg(linecount):

1

volume:

HIGH

splunkd

count:

267

avg(linecount):

1

volume:

HIGH

splunkd_ui_access

count:

222

avg(linecount):

1

volume:

HIGH

node:supervisor

count:

137

avg(linecount):

1.0072992700729928

volume:

MEDIUM

postgres

count:

58

avg(linecount):

1.0172413793103448

volume:

MEDIUM

---

### Result Interpretation

The search returned 1014 events matching the search term:

error

The stats command grouped the events by:

sourcetype

For each sourcetype, stats calculated:

count

The number of events in the sourcetype group.

avg(linecount)

The average linecount value within the sourcetype group.

The eval command used the count field to classify each statistical result row.

The case() conditions were:

count > 200 -> HIGH

count > 50 -> MEDIUM

otherwise -> LOW

The node:sidecar:postgres:stdout group had count = 295 and was classified as HIGH.

The splunkd group had count = 267 and was classified as HIGH.

The splunkd_ui_access group had count = 222 and was classified as HIGH.

The node:supervisor group had count = 137 and was classified as MEDIUM.

The postgres group had count = 58 and was classified as MEDIUM.

The remaining groups had counts below 50 and were classified as LOW.

---

### Execution Flow

index=_internal
    ->
search error
    ->
1014 matching events
    ->
stats count avg(linecount) by sourcetype
    ->
10 statistical result rows
    ->
count and avg(linecount) fields created
    ->
eval case() evaluates count
    ->
HIGH / MEDIUM / LOW classification
    ->
sort - count

---

### Key Learning

stats can create multiple aggregation fields in one command.

count measures the number of events in each sourcetype group.

avg(linecount) calculates the average linecount within each sourcetype group.

eval can use fields created by a previous stats command.

case() classifies the statistical result rows based on the count field.

sort - count orders the statistical rows by count.

sort does not create or change the volume classification.

The classification is created by eval and case() before sort runs.

---

### Troubleshooting / Verification

Search completed successfully.

No event sampling was reported.

The Statistics tab was selected.

The result contained 10 statistical rows.

The node:sidecar:postgres:stdout group contained 295 events and was classified as HIGH.

The node:supervisor group contained 137 events and was classified as MEDIUM.

The classification was based on count, not avg(linecount).

sort - count only ordered the results and did not assign the classification.

---

### Lab Status

COMPLETE

Understanding:

9 / 10

---

## Module 02 Lab Progress

Lab 1:

COMPLETE

Lab 2:

COMPLETE

Lab 3:

COMPLETE

Lab 4:

COMPLETE

Lab 5:

COMPLETE

Lab 6:

COMPLETE

Lab 7:

COMPLETE

Lab 8:

COMPLETE

Lab 9:

COMPLETE

Lab 10:

COMPLETE

Next:

Formal Questions
