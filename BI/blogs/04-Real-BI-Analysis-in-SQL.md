# Real BI Analysis in SQL: Funnels, Cohorts, Retention & Common Pitfalls

This post walks through **common BI analysis patterns in SQL**: things like funnels, cohorts, and retention.

The goal isn’t just to show queries, but to show **how analysts think when working on real business problems**, and how to avoid the kinds of mistakes that quietly break metrics.

![SQL for BI](../assets/azure_2_sqltopbi.png)

---

## 1. Why patterns matter

In real BI work, you rarely invent a brand-new type of analysis.

Most dashboards and business questions are built on a few repeatable patterns:

* **Funnels** → how users move through a flow (visit → signup → purchase)
* **Cohorts** → how groups of users behave over time (e.g. signup week)
* **Retention** → whether users come back after their first interaction

Once you understand these patterns, you can answer a large portion of business questions with **clear and reliable SQL**.

---

## 2. Funnel Analysis

A funnel tracks how users move through a sequence of steps.

Example:

> visit → signup → purchase

### Example Table: `events`

| user_id | event    | event_time       |
| ------- | -------- | ---------------- |
| 1       | visit    | 2024-06-01 10:00 |
| 1       | signup   | 2024-06-01 10:10 |
| 1       | purchase | 2024-06-01 10:15 |
| 2       | visit    | 2024-06-01 10:01 |
| 2       | visit    | 2024-06-01 10:02 |

### SQL: Count users at each step

```sql id="7i5c1y"
SELECT
  'visit' AS step,
  COUNT(DISTINCT user_id) AS users
FROM events WHERE event = 'visit'

UNION ALL

SELECT
  'signup',
  COUNT(DISTINCT user_id)
FROM events WHERE event = 'signup'

UNION ALL

SELECT
  'purchase',
  COUNT(DISTINCT user_id)
FROM events WHERE event = 'purchase';
```

### Drop-off and Conversion

To understand where users drop off, calculate conversion between steps:

* signup rate = signup users / visit users
* purchase rate = purchase users / signup users

👉 Key rule:

> Always use the same grain (usually `user_id`) across all steps.

### 💡 Small but important nuance

A “correct” funnel often needs **ordering**:

* Did the user sign up *after* visiting?
* Did purchase happen *after* signup?

Without enforcing order, you may count users who completed steps in the wrong sequence.

---

## 3. Cohort Analysis

Cohort analysis groups users by a shared starting point, then tracks how they behave over time.

A common example:

> Do users who signed up this week behave differently from last week’s users?

### Example: Activity by signup week

```sql id="s1b4kq"
WITH first_signup AS (
  SELECT 
    user_id, 
    MIN(DATE_TRUNC('week', event_time)) AS cohort_week
  FROM events
  WHERE event = 'signup'
  GROUP BY user_id
),
user_activity AS (
  SELECT 
    e.user_id, 
    DATE_TRUNC('week', e.event_time) AS activity_week
  FROM events e
)
SELECT
  f.cohort_week,
  u.activity_week,
  COUNT(DISTINCT u.user_id) AS active_users
FROM first_signup f
JOIN user_activity u 
  ON f.user_id = u.user_id
WHERE u.activity_week >= f.cohort_week
GROUP BY f.cohort_week, u.activity_week
ORDER BY f.cohort_week, u.activity_week;
```

### How to read this

* Each row = one cohort (signup week) at one point in time
* You can track how engagement changes week by week
* This is the foundation for retention curves

### 💡 Important detail

Cohort analysis only works if:

* your **cohort definition is stable** (first signup, first purchase, etc.)
* your **time buckets are consistent** (week, month, etc.)

Small inconsistencies here can completely distort trends.

---

## 4. Retention Analysis

Retention answers a simple question:

> After users join, do they come back?

### Steps

1. Find each user’s first activity (signup date)
2. Check whether they return after N days

### Example: Day 1 / 7 / 30 retention

```sql id="q9u3zv"
WITH first_signup AS (
  SELECT 
    user_id, 
    MIN(DATE(event_time)) AS signup_date
  FROM events
  WHERE event = 'signup'
  GROUP BY user_id
),
subsequent_activity AS (
  SELECT
    e.user_id,
    DATE(e.event_time) AS activity_date
  FROM events e
)
SELECT
  n_days_later,
  COUNT(DISTINCT user_id) AS retained_users
FROM (
  SELECT
    f.user_id,
    DATEDIFF(a.activity_date, f.signup_date) AS n_days_later
  FROM first_signup f
  JOIN subsequent_activity a 
    ON f.user_id = a.user_id
  WHERE a.activity_date > f.signup_date
) t
WHERE n_days_later IN (1, 7, 30)
GROUP BY n_days_later
ORDER BY n_days_later;
```

### 💡 What matters here

* You’re always comparing **activity relative to a starting point**
* Retention is about *return behavior*, not total activity

---

## 5. Common BI SQL Mistakes

Even with correct patterns, small mistakes can break your results.

**Grain mismatch**

* Aggregating at the wrong level (event vs user)
* Example: `COUNT(user_id)` on event table → counts events, not users

**Double counting from joins**

* Joining fact tables can multiply rows
* This silently inflates metrics

👉 Always check row counts before and after joins

**Incorrect DISTINCT usage**

* `DISTINCT` can hide duplication, or introduce new issues
* Especially risky in multi-join queries

**Time filtering mistakes**

* Using the wrong timestamp column
* Ignoring timezones
* Mixing “created” vs “completed” events

These often cause subtle but significant discrepancies.

---

## 6. Validation Checklist (very important)

Before trusting any BI query, run through a quick sanity check:

* **Row count sanity**
  Does the number roughly match expectations?

* **Metric definition**
  Does the query match how the business defines the metric?

* **Join behavior**
  Did any join multiply rows unexpectedly?

* **Time boundaries**
  Are your filters aligned with the reporting window?

### Extra (highly recommended)

If possible:

* compare against a known report
* check previous periods (week/month)
* validate with a small manual sample

---

## 💡 Key Takeaway

> BI SQL isn’t about writing clever queries—it’s about writing queries you can trust.

Focus on:

* clear definitions
* consistent grain
* careful joins
* and validating results

Because at the end of the day, these numbers drive real decisions.
