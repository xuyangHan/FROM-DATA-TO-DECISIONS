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

👉 If you skip this step, your aggregations can silently break.

---

## 3. How to Tackle Missing Data

Missing values are unavoidable. What matters is how you handle them.

### Nulls are not zeros

A missing value doesn’t mean “0”, but it could mean:

- unknown
- not applicable
- not recorded

Treating them as zeros without thinking can distort your metrics.

### Drop vs Fill

Two common approaches:

**Drop rows**
``` python
df = df.dropna(subset=['Revenue'])
```
Use this when missing data is rare and not critical.

**Fill values**
``` python
df['Revenue'] = df['Revenue'].fillna(0)

# or fill with an average
df['Revenue'] = df['Revenue'].fillna(df['Revenue'].mean())
```

Use this when you need to keep rows, but understand the impact on your analysis.

👉 There’s no “correct” answer, only trade-offs.

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

### Multiple aggregations

```python id="w9c2qe"
agg = df.groupby('Product').agg(
    sales_count=('OrderID', 'count'),
    total_revenue=('Revenue', 'sum'),
    avg_price=('Revenue', 'mean')
)
```

This is how you build most KPIs in Python—very similar to SQL, just more flexible.

---

## 5. Reshaping Data: Wide vs Long

Sometimes your data is in the wrong shape for analysis.

### Pivot (wide format)

```python id="p4v7hj"
monthly = df.pivot_table(
    index='Customer',
    columns='Month',
    values='Revenue',
    aggfunc='sum'
)
```

### Melt (long format)

```python id="k8m1rt"
long_df = df.melt(
    id_vars=['Customer'],
    var_name='Month',
    value_name='Revenue'
)
```

Think of this like Excel PivotTables, but fully repeatable in code.

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

👉 The key idea:

> Your cleaning steps become a system—not a one-off task.

---

## 💡 Key takeaway

> Clean data isn’t a one-time step, but it’s a repeatable process.

The better your data wrangling:

* the more reliable your metrics
* the easier your analysis becomes
* and the less time you spend fixing mistakes later
