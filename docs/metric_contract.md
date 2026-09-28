# FlowPay Metric Contract

## Business decision
The analysis will determine whether the 7 day activation rate has declined across weekly signup cohorts. If the decline is confirmed^ the analysis will identify where it is concentrated and which potential explanation thr Product Manager should priorities for further investigation or product testing.

## Primary metric

### Name
Activation_7d_rate

### Business meaning
The seven-day activation rate is the percentage of eligible new users who completed at least one successful transfer within 168 hours after registration. 

A successful transfer represents the first measurable moment when a new user received the core value from FlowPay. Comparing this metric across weekly signup cohort allows the Product team to identify changes in early customer success over time.

### Unit of analysis
The unit of analysis is one eligible registered user. The analytical dataset will contain exactly one row per eligible user, including users with no events, verification attempts, transfers or support contacts.

### Eligible population
Registered non-internal users with a valid signup timestamp and a complete 168-hour observation window before the analysis snapshot.

### Numerator
The number of eligible registered users who completed at least one successful transfer within the first 168 hours after their signup timestamp. Each user is counted once, regardless of the number od successful transfers.

### Denominator
All eligible registered users, including users with no recorded events, verification attempts or transfers.

### Observation window
For each eligible user, the observation window starts at 'signup_timestamp' aand ends 168h later. The metric uses the half-open interval: 
`[signup_timestamp, signup_timestamp + 168 hours)`

### Analysis snapshot
The analysis uses an assumed snapshot of `2026-08-04 00:00:00 UTC`. This is a documented assumption for the synthetic dataset rather than a pipeline-confirmed extraction timestamp.

### Cohort maturity rule
A user is eligible only when:
`signup_timestamp + 168 hours <= analysis_snapshot`

Equivalently:
`signup_timestamp <= analysis_snapshot - 168 hours`

With an assumed snapshot of `2026-08-04 00:00:00 UTC`, users who signed up after `2026-07-28 00:00:00 UTC` would not have a complete observation window and would be excluded.

### Source of truth

### Exclusions

### Boundary conditions

### Data-quality treatment

### Known limitations