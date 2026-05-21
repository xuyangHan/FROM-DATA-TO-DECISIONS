# Data Wrangling in Python: Turning Raw Data into Trustworthy Insights

Data cleaning isn’t just a boring chore, but it’s what makes every dashboard and analysis *trustworthy*.

In BI, a huge part of the job isn’t analyzing data, but it’s getting the data into a shape where analysis actually makes sense.

---

## 1. Why Data Wrangling Is *Actually* the Real BI Job

You’ve probably seen the joke:

> “I thought I’d be doing analysis… but I spend all day cleaning data.”

It’s funny because it’s true.

In real-world BI work, you almost never start with a clean dataset. Instead, you get:

* **Messy data** → missing values, duplicates, inconsistent formats
* **Mixed sources** → SQL exports, Excel files, CSVs, APIs—all slightly different

*Data wrangling* is the process of turning that mess into something structured, consistent, and reliable, so your metrics and dashboards actually mean what you think they mean.

---

## 2. Core Pandas Operations You’ll Use All the Time

Python’s **pandas** library is built for this kind of work.

If Excel is your starting point, think of pandas as Excel, but **scriptable, scalable, and repeatable**.

Here are the core operations you’ll use constantly:

### Filtering Rows

Keep only the data you care about:

```python id="r1v8sj"
df = df[df['Country'] == 'USA']     # only US rows
df = df[df['Revenue'] > 1000]       # only large deals
```

### Selecting Columns

Focus on the fields you actually need:

```python id="8n2p4m"
df = df[['Date', 'Revenue', 'Product']]
```

### Sorting

Quickly find top performers or recent activity:

```python id="x7k3hd"
df = df.sort_values('Revenue', ascending=False)
```

### Type Conversion

Data rarely comes in clean.

Numbers may be stored as text, dates as strings. Fix this before doing any analysis:

```python id="z4q1tw"
df['Revenue'] = pd.to_numeric(df['Revenue'], errors='coerce')
df['Date'] = pd.to_datetime(df['Date'])
```

If you skip this step, your aggregations can silently break.

---

## 3. How to Tackle Missing Data

Missing values are unavoidable. What matters is how you handle them.

### Nulls are not zeros

A missing value doesn’t mean “0”, but it could mean:

* unknown
* not applicable
* not recorded

Treating them as zeros without thinking can distort your metrics.

### Drop vs Fill

Two common approaches:

#### Drop rows

``` python
df = df.dropna(subset=['Revenue'])
```

Use this when missing data is rare and not critical.

#### Fill values

``` python
df['Revenue'] = df['Revenue'].fillna(0)

# or fill with an average
df['Revenue'] = df['Revenue'].fillna(df['Revenue'].mean())
```

Use this when you need to keep rows, but understand the impact on your analysis.

There’s no “correct” answer, only trade-offs.

---

## 4. Grouping & Aggregation: Pandas vs SQL

If you know SQL, this part will feel familiar.

In SQL:

```sql
GROUP BY Region
```

In pandas:

```python id="n6t3kp"
region_sales = df.groupby('Region')['Revenue'].sum()
```

Same idea:

> Split data into groups → calculate something for each group

### A tiny example 

Imagine your raw data looks like this:

```python 
   OrderID Region Product  user_id  Revenue
0        1   East       A       10      100
1        2   East       B       10      200
2        3   West       A       11       50
3        4   West       B       12      300
4        5   West       B       12      300
```

Now group by region and sum revenue:

```python id="grp_region_sales"
region_sales = df.groupby("Region")["Revenue"].sum()
region_sales
```

Result:

```python id="grp_region_sales_out"
Region
East    300
West    650
Name: Revenue, dtype: int64
```

When you write the grouping query, you’re really answering:

> “What does each row represent after aggregation?”

In this example, after aggregation, each row represents:

> **one region with its total revenue**

That “row meaning” (also called the *grain*) is the foundation of BI.
If the grain is unclear, your metric is unclear.

### Multiple metrics at once (common BI pattern)

```python id="w9c2qe"
agg = df.groupby('Product').agg(
    orders=('OrderID', 'count'),
    total_revenue=('Revenue', 'sum'),
    avg_order_value=('Revenue', 'mean')
)
```

Result:

```python id="grp_reset_index_out"
  Product  orders  total_revenue  avg_order_value
0       A       2            150             75.0
1       B       3            800            266.7
```

Now each row represents:

> **one product with multiple KPIs**

This is the foundation of most dashboards.

If you want the same thing as a “table” (not a Series), add `.reset_index()`:

```python id="grp_reset_index"
agg = agg.reset_index()
agg
```

### Common pitfall: counting the wrong thing

This is where people quietly get it wrong, especially with event-level tables.

```python id="grp_count_vs_nunique"
# counts non-null rows (events)
events = df.groupby("Region")["user_id"].count()

# counts unique users
unique_users = df.groupby("Region")["user_id"].nunique()

events, unique_users
```

This is the same mental model as SQL:

- `COUNT(*)` / `COUNT(user_id)` = “how many rows?”
- `COUNT(DISTINCT user_id)` = “how many unique users?”

If your dataset has multiple rows per user, `.count()` will overstate “users”.

### Grain still matters 

Grouping doesn’t magically fix grain problems. It only summarizes whatever you feed it.

If your data is at event level (multiple rows per order/user), then:

```python
df.groupby("Region")["Revenue"].sum()
```

can be wrong if:

- revenue is duplicated across rows (e.g., line items repeating order revenue)
- a join earlier created **fan-out** (one-to-many that multiplies revenue)

This is the same rule you already know from SQL:

> **Aggregation only works if your underlying data is at the correct grain.**

### Filtering before vs after grouping (subtle, but big meaning)

These two lines look similar, but answer different business questions:

```python id="grp_filter_before"
# Filter BEFORE grouping: "sum of large deals only"
df[df["Revenue"] > 100].groupby("Region")["Revenue"].sum()
```

```python id="grp_filter_after"
# Filter AFTER grouping: "regions whose total revenue is > 100"
df.groupby("Region")["Revenue"].sum().loc[lambda s: s > 100]
```

Small difference in code, big difference in interpretation.

---

## 5. Reshaping Data: Wide vs Long 

Before jumping into code, let’s make this concrete.

Data usually comes in two shapes:

### Wide format (spread out like Excel)

Example:

```text
Customer  Jan  Feb  Mar
A         100  200  150
B          80  120   90
```

Each column represents a time period or category.

This is common in:

- Excel reports
- exported dashboards
- finance-style tables

### Long format (one row per observation)

Example:

```text
Customer  Month  Revenue
A         Jan    100
A         Feb    200
A         Mar    150
B         Jan     80
```

Each row is one data point.

This is what most tools prefer:

- SQL tables
- pandas workflows
- visualization tools

### Why this matters in BI

Different tasks prefer different shapes:

- analysis and grouping are usually easier in **long format**
- reporting and presentation are often easier in **wide format**

In real BI work, you constantly switch between them, espeically if something feels hard to analyze.

### Pivot: Long → Wide

Turn rows into columns (like an Excel PivotTable):

```python id="p4v7hj"
df_wide = df.pivot_table(
    index='Customer',
    columns='Month',
    values='Revenue',
    aggfunc='sum'
)
```

Use this when:

- building reports
- creating summaries for stakeholders
- exporting to Excel

### Melt: Wide → Long

Turn columns back into rows:

```python id="k8m1rt"
df_long = df.melt(
    id_vars=['Customer'],
    var_name='Month',
    value_name='Revenue'
)
```

Use this when:

- preparing data for analysis
- feeding data into charts
- working with time-series or grouped calculations

### Common confusion

Beginners often:

- try to group wide data (awkward)
- or try to present long data directly (hard to read)

Reshaping fixes both problems.

---

## 6. Building a Repeatable Cleaning Pipeline

This is where Python really starts to shine.

In Excel:

* you click, filter, copy, paste
* repeat the same steps every week

In Python:

* you write the steps once
* and rerun them forever

### Example pipeline

```python id="u3d8zx"
import pandas as pd

df = pd.read_csv('input.csv')

df = df[df['Active'] == True]              # Filter
df['Date'] = pd.to_datetime(df['Date'])   # Type conversion
df = df.dropna(subset=['Revenue'])        # Drop missing
df['Revenue'] = df['Revenue'].fillna(0)   # Fill remaining

result = df.groupby('Region')['Revenue'].sum()

result.to_csv('output.csv', index=True)
```

**The key idea**: Your cleaning steps become a system—not a one-off task.

---

## 💡 Key takeaway

> Clean data isn’t a one-time step, but it’s a repeatable process.

Python helps by turning manual cleaning into reusable code:

- run the same logic every week with new data
- reduce copy-paste mistakes from spreadsheet workflows
- document your transformation steps so others can trust and reuse them

The better your data wrangling:

* the more reliable your metrics
* the easier your analysis becomes
* and the less time you spend fixing mistakes later
