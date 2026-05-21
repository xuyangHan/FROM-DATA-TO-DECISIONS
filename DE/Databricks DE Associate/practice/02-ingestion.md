# Domain 2 - Ingestion (practice)

Use these to test whether you can pick the right ingestion pattern, tool, and target from short exam-style scenarios.

## Questions and sample answers

1. 50,000 small JSON files arrive daily under `s3://logs/incoming/`. Governance requires UC table `main.bronze.logs`. Best ingestion tool?

   **Sample answer:** Auto Loader. It is built for incremental cloud-file ingestion at higher file volume and can land into a Unity Catalog Delta table.

2. A CSV dump lands once per week in ADLS with hundreds of files. The SQL team owns the pipeline. Simplest incremental approach?

   **Sample answer:** `COPY INTO` into a UC Delta table. It is SQL-first and idempotently skips files already loaded.

3. Salesforce data must sync to bronze with minimal custom code. A managed Lakeflow Connect connector exists. What should you choose?

   **Sample answer:** Lakeflow Connect managed connector. Prefer the managed connector over custom REST or JDBC code when it supports the source and requirement.

4. A one-off REST API returns a 2 MB JSON response every hour. Lakeflow Connect is not available. What pattern fits?

   **Sample answer:** Use a notebook or Python task with a REST client, convert the response to a DataFrame, write to a UC bronze Delta table, and schedule it with Lakeflow Jobs.

5. Compare Auto Loader and `COPY INTO` for millions of files per day.

   **Sample answer:** Auto Loader is the better fit. `COPY INTO` works for simpler batch file loads, but Auto Loader scales better for millions of files, frequent arrivals, schema drift, and file notification patterns.

6. JSON files add new optional fields over time. Which Auto Loader settings matter?

   **Sample answer:** Use `cloudFiles.schemaLocation`, `cloudFiles.schemaEvolutionMode`, `cloudFiles.schemaHints` if needed, `rescuedDataColumn`, and `mergeSchema` on the write when evolving the target table.

7. A PostgreSQL table is 500 GB and partitionable on `id`. A nightly notebook extracts it. Which JDBC options parallelize the read?

   **Sample answer:** Use `partitionColumn`, `lowerBound`, `upperBound`, and `numPartitions`. Use a source-side `WHERE` clause if you also need to filter rows.

8. Semi-structured SaaS events have nested `payload.tags`. Data lands in bronze via Lakeflow Connect. What PySpark operation can flatten the tags array?

   **Sample answer:** Use `explode` or `explode_outer`. Prefer doing this in silver so bronze preserves the raw source shape.

9. You need Unity Catalog lineage on landing tables. What must be true of the target?

   **Sample answer:** The target should be a UC-governed table using a three-part name like `catalog.schema.table`, not only files in DBFS or an unregistered path.

10. When is a partner marketplace connector acceptable instead of Lakeflow Connect?

    **Sample answer:** When first-party Lakeflow Connect does not support the source or the scenario explicitly requires a partner tool. If a supported Databricks connector exists, prefer it.

11. A single local CSV is provided for an ad hoc analysis. What is the simplest ingestion path?

    **Sample answer:** Use the Add data UI to create a table, or upload the file to a Unity Catalog volume and read it into a bronze table.

12. A recurring large local file feed must become a production ingestion pipeline. Is manual file upload enough?

    **Sample answer:** No. Stage the files in governed cloud storage or a UC volume, then use Auto Loader or `COPY INTO`, scheduled through Jobs or a pipeline.

13. A stream uses `spark.readStream` and writes to Delta. What option is required for fault tolerance?

    **Sample answer:** `checkpointLocation`. Each stream needs its own unique checkpoint path.

14. Auto Loader is using schema inference and schema evolution. What is `cloudFiles.schemaLocation` for?

    **Sample answer:** It stores inferred schema metadata and schema changes. It is separate from the streaming checkpoint.

15. What is the difference between `checkpointLocation` and `cloudFiles.schemaLocation`?

    **Sample answer:** `checkpointLocation` tracks streaming progress and discovered files. `cloudFiles.schemaLocation` tracks inferred/evolved schemas.

16. An Auto Loader job should process all files currently available and then stop. Which trigger should it use?

    **Sample answer:** `Trigger.AvailableNow`, written in PySpark as `.trigger(availableNow=True)`.

17. Files arrive continuously and low-latency micro-batches are required. Should you use `AvailableNow` or a processing-time trigger?

    **Sample answer:** Use a processing-time trigger such as `.trigger(processingTime="1 minute")`. `AvailableNow` processes current files and stops.

18. A SQL team wants a scalable file ingestion approach, but file volume is growing beyond simple `COPY INTO`. What Databricks SQL pattern can use Auto Loader?

    **Sample answer:** A streaming table with `read_files`, for example `CREATE OR REFRESH STREAMING TABLE ... AS SELECT * FROM STREAM read_files(...)`.

19. A JSON file has new unexpected fields and malformed values. You do not want to lose them. What Auto Loader feature helps?

    **Sample answer:** Configure a rescued data column such as `_rescued_data`, often with schema evolution settings.

20. A production Auto Loader stream fails after a new column appears under `addNewColumns`. Is that always bad?

    **Sample answer:** No. With `addNewColumns`, the stream can fail intentionally so the schema is updated; after restart or retry, it can continue with the new column.

21. A team wants strict production schema and the stream should fail on unknown new columns. Which schema evolution mode fits?

    **Sample answer:** `failOnNewColumns`.

22. What is the main danger of using the same checkpoint path for two different streams?

    **Sample answer:** It can corrupt or confuse streaming state. Each distinct stream/source/target should have a unique checkpoint.

23. In `COPY INTO`, what does `force = true` do?

    **Sample answer:** It reloads files even if they were loaded before. This can create duplicates if the downstream table is append-only.

24. Does `COPY INTO` upsert existing rows by primary key?

    **Sample answer:** No. `COPY INTO` loads files. If you need upserts, land data first, then use `MERGE` or downstream dedup logic.

25. You need to load only two known files from a folder with `COPY INTO`. Which clause helps?

    **Sample answer:** `FILES = (...)`. Use `PATTERN` for glob-style matching instead.

26. You want to validate `COPY INTO` parsing and schema before writing rows. Which clause helps?

    **Sample answer:** `VALIDATE`, such as `COPY INTO ... VALIDATE ALL`.

27. A cloud path is not covered by a UC external location and the user lacks file access. What happens to `COPY INTO`?

    **Sample answer:** It fails with access errors. Use a Unity Catalog volume or an external location with the required privileges.

28. A source is MySQL with CDC enabled and a managed database connector is supported. What should you choose?

    **Sample answer:** Lakeflow Connect managed database CDC connector.

29. A database has no CDC setup but has an `updated_at` cursor column. Which Lakeflow Connect pattern may fit?

    **Sample answer:** A query-based connector, which tracks a high-water mark or cursor column on a schedule.

30. A database CDC connector needs to reach an on-prem database. What non-code requirement matters?

    **Sample answer:** Network connectivity from the ingestion gateway to the database, such as VPN, Direct Connect, ExpressRoute, peering, or an allowed public endpoint.

31. In Lakeflow Connect, what is a Unity Catalog connection?

    **Sample answer:** A UC securable object that stores endpoint, credentials, owner, and configuration for an external system.

32. What is the difference between query federation and Lakeflow Connect ingestion?

    **Sample answer:** Query federation reads external data in place. Lakeflow Connect ingests data into Delta tables in Databricks.

33. A custom REST API returns 5 GB of JSON per run. Why is one giant `requests.get(...).json()` risky?

    **Sample answer:** It can cause driver memory pressure or OOM. Use pagination, streaming download, or write raw pages/files to storage and ingest with Auto Loader.

34. A REST API has rate limits and sometimes returns HTTP 429. What should a production notebook include?

    **Sample answer:** Retry and backoff logic, smaller pages, failure handling, and Lakeflow Job retries/alerts.

35. A JDBC extract uses `lowerBound` and `upperBound`. Do those options filter rows?

    **Sample answer:** No. They define partition stride for parallel reads. Use a source SQL `WHERE` clause to filter rows.

36. A JDBC table is large, but the source database is fragile. Should you set `numPartitions` very high?

    **Sample answer:** No. Too many partitions create too many parallel connections and can overload the source. Tune carefully.

37. A bronze table receives append-only snapshots from JDBC. How can silver keep only the latest row per key?

    **Sample answer:** Use a window with `ROW_NUMBER()` over the business key ordered by `updated_at` descending, then keep `rn = 1`, or use Delta `MERGE`.

38. Which task type should a scheduled custom ingestion notebook use in Lakeflow Jobs?

    **Sample answer:** A notebook task, with parameters such as `run_date`, `start_ts`, or `end_ts`, running on serverless jobs compute or classic jobs compute as needed.

39. Where should database passwords and API tokens be stored?

    **Sample answer:** In Databricks secrets or governed UC connections, not plaintext notebook variables.

40. A source contains PDFs and images. Should these be stored directly as normal Delta table columns?

    **Sample answer:** Usually no. Store the files in a Unity Catalog volume and create a Delta metadata table with paths, sizes, labels, source info, and processing status.

41. JSON payload fields are stable and heavily queried by BI users. Should you leave all fields only in a JSON string?

    **Sample answer:** No. Extract frequently queried fields into typed columns or structs for easier querying and better performance.

42. JSON payload schema changes often and has many rarely used fields. Which storage type may help on newer runtimes?

    **Sample answer:** `VARIANT`, possibly with important fields extracted as normal columns.

43. Which function parses a JSON string column into a typed struct?

    **Sample answer:** `from_json(col, schema)`.

44. Which function parses a JSON string into a `VARIANT` value?

    **Sample answer:** `parse_json`.

45. What is the difference between `explode` and `explode_outer`?

    **Sample answer:** `explode` drops rows with null or empty arrays. `explode_outer` preserves rows when the array is null, emitting null for the exploded value.

46. Should you flatten every nested field in bronze?

    **Sample answer:** No. Bronze should preserve raw source fidelity. Flatten and type fields in silver based on downstream needs.

47. A Kafka event has a JSON payload string. What is the general ingestion and parsing pattern?

    **Sample answer:** Use a standard connector with Structured Streaming or Lakeflow Spark Declarative Pipelines, then parse the payload with `from_json` or `parse_json`.

48. A question mentions "only new or changed records since the last successful run." What kind of ingestion pattern is this?

    **Sample answer:** Incremental ingestion. The state may come from a checkpoint, loaded-file tracking, CDC logs, a cursor column, or an audit/watermark table.

49. A nightly pipeline overwrites bronze with the latest full snapshot and loses prior raw records. What is the exam concern?

    **Sample answer:** Bronze should usually preserve raw history or source fidelity. Overwriting bronze can break replay, audit, and debugging unless the scenario explicitly calls for full snapshot replacement.

50. In one sentence, what is the most important Domain 2 rule?

    **Sample answer:** Identify the source and arrival pattern first, then choose the most managed ingestion method that lands data into Unity Catalog-governed Delta tables or volumes.
