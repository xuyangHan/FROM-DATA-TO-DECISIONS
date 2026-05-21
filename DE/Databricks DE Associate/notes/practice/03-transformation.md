# Domain 3 - Transformation (practice)

Use these to test whether you can choose the right transformation, modeling, quality, and tuning pattern from short exam-style scenarios.

## Questions and sample answers

1. Bronze `orders_raw` must become cleaned silver without losing raw history. What is the expected pattern?

   **Sample answer:** Read from `main.bronze.orders_raw`, apply cleaning and validation, then write a new Delta table such as `main.silver.orders`. Do not overwrite bronze in place.

2. A bronze table has `order_ts_raw` as a string. Silver needs a timestamp column. Which function should you use?

   **Sample answer:** Use `to_timestamp(order_ts_raw, pattern)` in PySpark or SQL, then validate rows where parsing produced null.

3. A required business key `order_id` is null in some bronze rows. What should silver do?

   **Sample answer:** Filter or quarantine those rows before writing silver. Required null keys should not normally reach curated silver tables.

4. A numeric amount arrives as strings like `$1,203.50`. What is the cleaning pattern?

   **Sample answer:** Remove formatting characters with `regexp_replace`, cast to a numeric type such as `DECIMAL`, and validate cast failures.

5. A source email column has inconsistent casing and whitespace. Which transformations standardize it?

   **Sample answer:** Use `lower(trim(email))`.

6. You cleaned bronze orders and need the table governed by Unity Catalog. How should you save it?

   **Sample answer:** Write it as a UC table with a three-part name, for example `saveAsTable("main.silver.orders")`, not only to an unregistered path.

7. Fact table has 2 billion rows and dimension table has 10 thousand rows. Join on `product_id`. What optimization fits?

   **Sample answer:** Broadcast the small dimension table, either with `broadcast(dim)` in PySpark or a SQL broadcast hint, assuming it fits in memory.

8. You need all orders even when the customer dimension is missing. Which join type?

   **Sample answer:** Left join from orders to customers. Missing dimension attributes will be null.

9. You need only orders that have a matching customer record. Which join type?

   **Sample answer:** Inner join.

10. You need to find orders whose `customer_id` does not exist in the customer dimension. Which join type is useful?

    **Sample answer:** Use a left anti join from orders to customers on `customer_id`.

11. Two DataFrames must join on both `customer_id` and `region`. How do you express multiple keys in PySpark?

    **Sample answer:** Use `df1.join(df2, on=["customer_id", "region"], how="inner")`.

12. A join unexpectedly doubles row count. What is a likely cause?

    **Sample answer:** Duplicate join keys on one or both sides caused row multiplication. Check key uniqueness before the join.

13. A SQL query accidentally omits its join condition between two large tables. What happens?

    **Sample answer:** It creates a cross join or Cartesian product, producing `left_count * right_count` rows and likely very poor performance.

14. Stack `events_2025` and `events_2026` DataFrames with the same columns but possible column-order differences. Which API is safer?

    **Sample answer:** Use `unionByName`, because it aligns columns by name instead of position.

15. Stack two event DataFrames where the newer table has extra optional columns. Which PySpark API helps?

    **Sample answer:** Use `unionByName(..., allowMissingColumns=True)`, which fills missing columns with null.

16. Does PySpark DataFrame `union()` remove duplicates?

    **Sample answer:** No. PySpark DataFrame `union()` keeps duplicates. Use `dropDuplicates` afterward if deduplication is required.

17. In SQL, what is the difference between `UNION` and `UNION ALL`?

    **Sample answer:** `UNION` removes duplicate rows. `UNION ALL` keeps all rows, including duplicates.

18. You need to add a derived column `order_year` to a DataFrame. Which PySpark method is common?

    **Sample answer:** Use `withColumn("order_year", expr)` or select the derived expression with an alias.

19. What happens if `withColumn("amount", ...)` uses an existing column name?

    **Sample answer:** It replaces the existing `amount` column.

20. A source column is named `userId`, but silver uses `user_id`. Which method can rename it?

    **Sample answer:** Use `withColumnRenamed("userId", "user_id")`, or select `col("userId").alias("user_id")`.

21. You need to remove a raw payload column from a silver DataFrame. Which method?

    **Sample answer:** Use `drop("raw_payload")`, or select only the columns you want to keep.

22. Are `filter` and `where` different in PySpark DataFrames?

    **Sample answer:** No. They are equivalent aliases for row filtering.

23. A string column `path` contains values like `/shop/cart/item`. Which function splits it into an array?

    **Sample answer:** Use `split(col("path"), "/")` in PySpark or `SPLIT(path, '/')` in SQL.

24. A column `tags` is an array and you need one row per tag. Which function?

    **Sample answer:** Use `explode(tags)`.

25. A row has `tags = null`. What does regular `explode(tags)` do?

    **Sample answer:** It returns no row for that input row. Use `explode_outer` if the parent row must be preserved.

26. After exploding `tags`, the row count increases. What changed?

    **Sample answer:** The grain changed from one row per event to one row per event-tag combination.

27. Remove duplicate `order_id` rows and keep an arbitrary row. Which PySpark method?

    **Sample answer:** Use `dropDuplicates(["order_id"])`.

28. You must keep the latest row per `customer_id` based on `updated_at`. Is `dropDuplicates(["customer_id"])` enough?

    **Sample answer:** No. Use a window with `row_number()` partitioned by `customer_id` and ordered by `updated_at DESC`, then keep `rn = 1`.

29. How do you find duplicate keys in SQL?

    **Sample answer:** Group by the key and use `HAVING COUNT(*) > 1`, for example `SELECT order_id, COUNT(*) FROM orders GROUP BY order_id HAVING COUNT(*) > 1`.

30. You need count, revenue sum, and average order amount by `order_date`. What DataFrame pattern fits?

    **Sample answer:** Use `groupBy("order_date").agg(count("*"), sum("amount"), avg("amount"))`.

31. Billion-row `user_id` cardinality is needed for an approximate dashboard metric. Which function?

    **Sample answer:** Use `approx_count_distinct("user_id")`.

32. When should you use exact `countDistinct` instead of `approx_count_distinct`?

    **Sample answer:** Use exact `countDistinct` when exact accuracy is required and the data size/cardinality is manageable.

33. In SQL, what is the difference between `COUNT(*)` and `COUNT(column)`?

    **Sample answer:** `COUNT(*)` counts all rows. `COUNT(column)` counts only rows where that column is not null.

34. Do `avg` and `mean` include null values?

    **Sample answer:** No. They ignore nulls.

35. What is `summary()` useful for on a DataFrame?

    **Sample answer:** Quick profiling statistics such as count, mean, standard deviation, min, percentiles, and max. It is usually exploratory, not a production gold metric.

36. Spark UI shows one task 20 times slower than other tasks in a shuffle stage. What is the likely problem?

    **Sample answer:** Data skew. One fix is to inspect key distribution and use techniques such as salting, repartitioning, or skew-aware join handling.

37. Spark UI shows high shuffle read/write during a join. What kind of operation is causing this?

    **Sample answer:** A wide transformation, likely a shuffle join. Consider broadcast for a small dimension, better join keys, or tuning shuffle partitions after measuring.

38. Which setting controls the default number of partitions after SQL/DataFrame shuffles?

    **Sample answer:** `spark.sql.shuffle.partitions`.

39. A small aggregation creates 200 tiny shuffle tasks and scheduling overhead dominates. What setting might you lower?

    **Sample answer:** Lower `spark.sql.shuffle.partitions`, then rerun and compare performance.

40. A huge shuffle stage has very large tasks and disk spill. What setting might you increase?

    **Sample answer:** Increase `spark.sql.shuffle.partitions` so each task handles less data, then re-measure. Also check skew and memory.

41. Which setting controls automatic broadcast join eligibility by table size?

    **Sample answer:** `spark.sql.autoBroadcastJoinThreshold`.

42. A large-small join is not broadcasting the small table. Name two possible actions.

    **Sample answer:** Use a broadcast hint or increase `spark.sql.autoBroadcastJoinThreshold` if the small table safely fits in executor memory. Fresh table statistics can also help.

43. A notebook does `df.collect()` on millions of rows and the driver crashes. Which memory area is involved?

    **Sample answer:** Driver memory. Avoid collecting huge datasets to the driver.

44. Executors fail with out-of-memory errors during large tasks. Which memory setting is relevant?

    **Sample answer:** `spark.executor.memory`, though the query may also need repartitioning, reduced shuffle size, or better skew handling.

45. What should you do after changing a Spark tuning parameter?

    **Sample answer:** Re-run the same workload on comparable input and compare Spark UI or Query Profile metrics such as runtime, shuffle, spill, and task skew.

46. Executives need pre-aggregated daily revenue queried 100 times per day, and source data updates hourly. Table, view, materialized view, or streaming table?

    **Sample answer:** A materialized view is a strong fit because the expensive aggregate is repeatedly queried and can refresh as data updates. A gold table refreshed hourly could also work if the pipeline controls refresh.

47. Analysts need a lightweight rename/join layer that should always reflect current silver data. Which gold object fits?

    **Sample answer:** A view.

48. A BI team needs a stable, physically stored curated fact table with documented grain. Which object fits?

    **Sample answer:** A managed Delta table in the gold schema.

49. A dashboard needs low-latency updates from an incoming stream. Which gold object may fit?

    **Sample answer:** A streaming table managed by Lakeflow Spark Declarative Pipelines.

50. Is a temporary view a good enterprise gold object for BI users?

    **Sample answer:** No. Temporary views are session-scoped and not governed/shared like Unity Catalog tables, views, materialized views, or streaming tables.

51. Where should gold BI objects usually be registered?

    **Sample answer:** In Unity Catalog using a schema such as `main.gold`, with appropriate permissions for BI users.

52. A gold dashboard should not expose raw PII from bronze. What should you use?

    **Sample answer:** Curate the data in silver/gold and apply Unity Catalog permissions, masking, row filters, or restricted views as appropriate.

53. A quality rule says `amount >= 0` for all future writes to a Delta table. What table feature can enforce it?

    **Sample answer:** A Delta `CHECK` constraint, for example `ALTER TABLE ... ADD CONSTRAINT valid_amount CHECK (amount >= 0)`.

54. You add a `CHECK` constraint to a table that already contains invalid rows. What happens?

    **Sample answer:** The constraint addition fails because existing data must satisfy the constraint.

55. Are Unity Catalog primary key and foreign key constraints enforced for uniqueness and referential integrity?

    **Sample answer:** No. They are informational metadata, not enforced constraints.

56. Invalid records must be auditable instead of silently dropped. What pattern should you use?

    **Sample answer:** Split valid and invalid records, write valid rows to silver, and write invalid rows to a quarantine table with a reason and timestamp.

57. A pipeline should continue with valid rows but record data quality failures. Which Lakeflow expectation behavior fits?

    **Sample answer:** Use an expectation that warns or tracks metrics, or `expect_or_drop` if bad rows should be removed while the pipeline continues.

58. A hard quality rule says any invalid row must stop the pipeline. Which expectation behavior fits?

    **Sample answer:** Use a fail behavior such as `expect_or_fail`.

59. A silver table must validate that every `customer_id` exists in the customer dimension. Which transformation helps identify bad rows?

    **Sample answer:** Use a left anti join from orders to customers to find missing customer keys.

60. In one sentence, what is the most important Domain 3 rule?

    **Sample answer:** Preserve bronze raw history, build governed silver/gold Delta objects with clear grain and quality rules, and measure Spark performance before and after tuning.
