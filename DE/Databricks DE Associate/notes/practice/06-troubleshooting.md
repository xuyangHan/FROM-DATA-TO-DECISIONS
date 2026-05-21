# Domain 6 - Troubleshooting, Monitoring, and Optimization (practice)

Use these to test whether you can move from symptom to the right Databricks UI surface, interpret job/task status correctly, and choose the right fix for Spark, layout, cluster, library, and memory issues.

## Questions and sample answers

1. Job duration jumped from 12 minutes to 45 minutes over the last week. What UI place should you check first?

   **Sample answer:** Open the Lakeflow Jobs **Runs** tab and compare recent run history against historical successful runs.

2. A run is slow today. Why should you compare it to a baseline before tuning Spark?

   **Sample answer:** A single slow run might be transient or caused by more input data, retries, compute startup, or upstream delay. A baseline shows whether there is a real trend.

3. In run history, total job duration increased but only one task changed. What should you inspect next?

   **Sample answer:** Open that task's run details, then its logs, query profile, or Spark UI depending on task type.

4. The job is slow but every Spark task duration looks normal. What non-Spark causes should you consider?

   **Sample answer:** Compute acquisition/startup time, queued time, upstream task delay, retries, concurrency limits, source-system slowness, or schedule overlap.

5. A run succeeded but took much longer than normal. Why might "Succeeded" not mean "healthy"?

   **Sample answer:** It may have succeeded after retries, repair, larger input volume, or a slow task that still met correctness requirements.

6. The task `transform` failed and `publish` shows `Skipped`. What should you fix first?

   **Sample answer:** Fix the upstream failed `transform` task first. `publish` skipped because its dependency did not complete successfully.

7. Which Jobs UI view best answers "what blocked what?"

   **Sample answer:** The graph/DAG view.

8. Which Jobs UI view best answers "where did the time go?"

   **Sample answer:** The timeline view.

9. Which Jobs UI view is useful for sorting tasks by duration or filtering by failed status?

   **Sample answer:** The list view.

10. A downstream task is marked `Skipped`. Is that a successful task?

    **Sample answer:** No. It did not run. Check dependency status and the task's `Run if` condition.

11. What does "Succeeded with failures" mean for a multi-task job?

    **Sample answer:** Some tasks failed, but all leaf tasks succeeded, so the overall job run is reported as succeeded with failures.

12. A job has 20 runs this week and 5 failed. What is the failure rate?

    **Sample answer:** 25%.

13. After a failed multi-task job, what feature can rerun only the failed/skipped tasks and dependent tasks?

    **Sample answer:** A repair run.

14. Does a repair run use the old broken task settings or the current task settings?

    **Sample answer:** It uses the current job and task settings.

15. A job is pending for a long time before tasks run. Name two likely causes.

    **Sample answer:** Waiting for dependencies, max concurrent runs, workspace run limits, cluster startup, cloud capacity, or cluster request limits.

16. A Spark stage shows 2 TB shuffle read and 1 TB shuffle write. What kind of operations should you suspect?

    **Sample answer:** Wide operations such as joins, aggregations, distinct, repartition, or window functions.

17. A Spark stage has one task that runs 20 times longer than the others. What is the likely issue?

    **Sample answer:** Data skew.

18. In the Spark UI, max task duration is much higher than the 75th percentile duration. What does that indicate?

    **Sample answer:** Possible skew in that stage.

19. A stage spills heavily to disk. What does spill mean?

    **Sample answer:** Spark ran out of memory for intermediate data and moved data to disk, which is slower.

20. A stage has one long task and low cluster utilization near the end. Should your first answer be "add more workers"?

    **Sample answer:** No. More workers often do not fix one skewed straggler; inspect key skew and partition sizes.

21. Name two fixes for a skewed join.

    **Sample answer:** Broadcast the small side, salt hot keys, filter or process hot keys separately, or rely on AQE where applicable.

22. A large fact table joins to a small dimension table but the plan shows a shuffle join. What optimization might help?

    **Sample answer:** Use/update statistics or apply a broadcast hint if the dimension is small enough.

23. A Spark job spills because each partition is huge. Which tuning parameter might you increase?

    **Sample answer:** `spark.sql.shuffle.partitions`, then re-measure.

24. A Spark job creates thousands of tiny tasks and scheduling overhead dominates. Which direction might you tune shuffle partitions?

    **Sample answer:** Decrease the number of shuffle partitions, then re-measure.

25. A long stage has high input but little shuffle. What should you check?

    **Sample answer:** Source scan/I/O, filters, file pruning, table layout, partitioning/clustering, and whether the query reads too much data.

26. A stage has only one task. Name two possible causes.

    **Sample answer:** Poor parallelism, a single large input partition/file, `coalesce(1)`, or a non-parallel operation.

27. What does `Exchange` in a Spark SQL physical plan usually indicate?

    **Sample answer:** A shuffle boundary.

28. What does `BroadcastExchange` indicate?

    **Sample answer:** Spark is preparing to broadcast a smaller side of a join.

29. A new large Delta table is filtered heavily by `event_date` and `country`. What layout feature might help?

    **Sample answer:** Liquid clustering with `CLUSTER BY (event_date, country)`.

30. Why not partition a large table by high-cardinality `user_id` by default?

    **Sample answer:** It can create too many tiny partitions/directories and small files. Liquid clustering is often a better answer.

31. What SQL clause creates a liquid-clustered table?

    **Sample answer:** `CLUSTER BY`.

32. Does liquid clustering replace the need for all maintenance?

    **Sample answer:** No. Clustering is commonly applied through `OPTIMIZE` or predictive optimization.

33. What does `OPTIMIZE` do?

    **Sample answer:** It rewrites/compacts data files and, for liquid-clustered tables, incrementally clusters data by clustering keys.

34. What does `VACUUM` do?

    **Sample answer:** It removes old unreferenced data files after the retention window.

35. What does predictive optimization run on Unity Catalog managed tables?

    **Sample answer:** `OPTIMIZE`, `VACUUM`, and `ANALYZE`.

36. Can predictive optimization manage external tables?

    **Sample answer:** No. It supports Unity Catalog managed tables, not external tables.

37. Where can predictive optimization be enabled?

    **Sample answer:** At the account, catalog, or schema level, with inheritance.

38. A team already enabled predictive optimization. Should they also schedule frequent blanket `OPTIMIZE` jobs for the same tables?

    **Sample answer:** Usually no. Avoid duplicate maintenance unless there is a specific reason.

39. Cluster fails at startup after an init script change. Where is the first evidence?

    **Sample answer:** The compute event log, followed by init script logs.

40. What event log entries indicate init scripts ran?

    **Sample answer:** Entries such as `INIT_SCRIPTS_STARTED` and `INIT_SCRIPTS_FINISHED`.

41. A cluster library is uninstalled but still appears pending removal. What is likely required?

    **Sample answer:** Restart the cluster.

42. A notebook attached to a running cluster cannot import a newly installed cluster library. What might be needed?

    **Sample answer:** Start a new notebook session or restart/reattach as appropriate.

43. A job fails with `ModuleNotFoundError`. What should you check?

    **Sample answer:** Whether the Python package is installed in the active environment/session and on the compute used by the job.

44. A JVM job fails with `NoSuchMethodError`. What kind of issue is likely?

    **Sample answer:** A JAR or dependency version conflict.

45. `df.collect()` on 10 million rows caused OOM. Is that likely driver OOM or executor OOM?

    **Sample answer:** Driver OOM, because `collect()` returns all rows to the driver.

46. What is a better pattern than collecting millions of rows to the driver?

    **Sample answer:** Keep the work distributed: aggregate in Spark, write to a table, or inspect only a small `limit()` sample.

47. Executors are lost during a skewed aggregation stage. What should you inspect?

    **Sample answer:** Spark UI Stages/Executors, failed task logs, spill, task duration spread, and key distribution.

48. Should increasing driver memory fix executor OOM caused by skew?

    **Sample answer:** No. Fix skew/partitioning or executor memory; driver memory is the wrong side.

49. Should increasing workers fix driver OOM caused by `toPandas()`?

    **Sample answer:** No. The data is being brought to the driver. Reduce driver-side data or avoid `toPandas()` on large data.

50. In one sentence, what is the Domain 6 troubleshooting loop?

    **Sample answer:** Start in Jobs run history, identify the failing or slow task with the graph/timeline/list views, deep dive with Spark UI or logs, match the metric to the cause, fix one thing, and re-measure.
