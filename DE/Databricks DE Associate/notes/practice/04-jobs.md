# Domain 4 - Jobs (practice)

Use these to test whether you can choose the right Lakeflow Jobs task type, dependency, control-flow feature, and trigger from short exam-style scenarios.

## Questions and sample answers

1. Task B should run only if Task A succeeds. Task C should send an alert if Task A fails. Which Lakeflow Jobs features should you use?

   **Sample answer:** Configure Task B with a normal dependency on Task A, using the default `Run if = All succeeded`. Configure Task C to depend on Task A with `Run if = At least one failed`.

2. A gold table must be refreshed nightly using a SQL statement only. Which task type and compute should you choose?

   **Sample answer:** Use a SQL task running on a serverless or Pro SQL warehouse.

3. A dashboard must refresh only after the gold SQL task succeeds. What should the task order be?

   **Sample answer:** Configure the DAG as SQL gold refresh task -> dashboard refresh task, with the dashboard task depending on the SQL task.

4. A bronze Delta table receives unpredictable afternoon updates. The silver job should run after the table commits. Which trigger should you use?

   **Sample answer:** Use a table update trigger on the bronze table.

5. A job must run every day at 2 AM UTC regardless of whether new data arrived. Which trigger should you use?

   **Sample answer:** Use a scheduled trigger with the time zone set to UTC.

6. An extract task sometimes fails because of transient S3 read errors. What task configuration should you add?

   **Sample answer:** Add task retries with a retry interval, such as `max_retries` and `min_retry_interval_millis`. A timeout can also protect against stuck runs.

7. A notebook task needs to process data for a supplied run date. How should the job pass that date into the notebook?

   **Sample answer:** Define a job or task parameter such as `process_date`, pass it to the notebook task, and read it with `dbutils.widgets.get("process_date")`.

8. An ingest notebook counts rows and a downstream branch should run only when the count is greater than zero. Which control-flow pattern fits?

   **Sample answer:** Have the ingest notebook set a task value such as `row_count`, then use an If/else condition task like `{{tasks.ingest.values.row_count}} > 0`.

9. What is the difference between `Run if` and an If/else condition task?

   **Sample answer:** `Run if` controls a task based on upstream task outcomes such as success or failure. An If/else task branches based on a boolean expression, often using parameters or task values.

10. A task should always run after upstream tasks finish, whether they succeed or fail, to clean up temporary state. Which dependency condition fits?

    **Sample answer:** Use `Run if = All done`.

11. A notification task should run only when at least one upstream dependency fails. Which `Run if` option fits?

    **Sample answer:** Use `Run if = At least one failed`.

12. A downstream task should run only if every upstream dependency succeeds. Which `Run if` option fits?

    **Sample answer:** Use the default `Run if = All succeeded`.

13. A team wants to process the same validation notebook for 20 source tables. Which Lakeflow Jobs feature avoids manually creating 20 nearly identical tasks?

    **Sample answer:** Use a For each task with an input array of table names and one nested notebook task.

14. In a For each task, what does `{{input}}` usually refer to?

    **Sample answer:** It refers to the current item from the For each input array.

15. A For each input item is `{"table": "orders", "schema": "bronze"}`. How can the nested task reference the table name?

    **Sample answer:** Use `{{input.table}}` in the nested task parameter.

16. A For each loop runs against an external API with strict rate limits. Which setting should you be careful with?

    **Sample answer:** The For each concurrency setting. Keep concurrency low enough to avoid overloading the API.

17. A job task has been running for six hours because the source system stopped responding. What configuration can stop runaway execution?

    **Sample answer:** Configure `timeout_seconds` on the task or job.

18. A SQL query fails because a column name is wrong. Should you solve this with retries?

    **Sample answer:** No. Retries are for transient failures. A wrong column name is a deterministic code error and should be fixed.

19. A permission-denied error occurs when a task reads a Unity Catalog table. Should retries be the main fix?

    **Sample answer:** No. Fix the privileges or the job's Run as identity. Retrying will not grant permission.

20. A task writes a small status value that a later task needs. Which feature should it use?

    **Sample answer:** Use task values, for example `dbutils.jobs.taskValues.set(key="status", value="ok")`.

21. A value is known before the job run starts, such as `env = prod`. Should it be a job parameter or a task value?

    **Sample answer:** Use a job parameter. Task values are created during a running task.

22. A value is discovered during the run, such as a row count or list of changed tables. Should it be a job parameter or a task value?

    **Sample answer:** Use a task value.

23. A job has tasks A -> B -> C, and C depends only on B. If A fails, will C run?

    **Sample answer:** No. B will not succeed, so C's dependency chain is blocked unless custom `Run if` behavior is configured.

24. Two independent ingestion tasks load customers and products. They do not depend on each other. How should the DAG be configured?

    **Sample answer:** Leave them independent so they can run in parallel. Downstream tasks should depend only on the inputs they actually need.

25. A gold customer-orders mart requires both cleaned customers and cleaned orders. How should the dependencies be configured?

    **Sample answer:** The gold task should depend on both `silver_customers` and `silver_orders`.

26. What does DAG stand for, and what does it imply about job dependencies?

    **Sample answer:** Directed acyclic graph. Dependencies have direction and cannot form cycles.

27. Why is a cyclic dependency not allowed in a Lakeflow Jobs DAG?

    **Sample answer:** A cycle would mean tasks wait on each other forever. A DAG must be acyclic.

28. A PySpark ETL step cleans bronze orders into silver. Which task type is the natural fit?

    **Sample answer:** Use a notebook task, or a pipeline task if the logic is implemented in a Lakeflow Spark Declarative Pipeline.

29. A pure SQL transform builds a daily revenue table. Which task type is the natural fit?

    **Sample answer:** Use a SQL query or SQL file task on a SQL warehouse.

30. A published BI dashboard should refresh after upstream tables are ready. Which task type is the natural fit?

    **Sample answer:** Use a dashboard task, with dependencies on the upstream transform tasks.

31. A Lakeflow Spark Declarative Pipeline must run as one step in a broader job. Which task type fits?

    **Sample answer:** Use a pipeline task, assuming it is a triggered pipeline.

32. Why is a continuous pipeline usually not triggered from a pipeline task?

    **Sample answer:** Continuous pipelines are already designed to run continuously. Pipeline tasks are for triggered pipeline runs inside a job.

33. A SQL task is configured but no SQL warehouse is selected. What is the issue?

    **Sample answer:** SQL tasks require an appropriate SQL warehouse, such as serverless or Pro SQL warehouse.

34. A dashboard task is placed before the gold table refresh. What is the likely problem?

    **Sample answer:** The dashboard may refresh stale data. The dashboard task should depend on the gold refresh task.

35. A notebook task and a SQL task both need the same `process_date`. What is the cleanest approach?

    **Sample answer:** Define `process_date` as a job parameter and pass it into both tasks.

36. A job should ingest raw files when a vendor drops new files into a UC volume path. Which trigger should you use?

    **Sample answer:** Use a file arrival trigger on the Unity Catalog volume path.

37. A file arrival trigger monitors a path where files are overwritten with the same name. Is that a good readiness signal?

    **Sample answer:** No. File arrival triggers are meant for new files. Overwriting existing files with the same name is not a reliable new-arrival signal.

38. A file drop arrives as 100 files over 10 minutes. The job should start only after the batch is quiet. What setting helps?

    **Sample answer:** Use `Wait after last change` on the file arrival trigger.

39. Can a file arrival trigger path use wildcards such as `*`?

    **Sample answer:** No. Configure a specific UC volume or external location path. The trigger can check subdirectories recursively.

40. What Databricks governance feature is required for file arrival triggers?

    **Sample answer:** Unity Catalog. File arrival triggers monitor Unity Catalog volumes or external locations.

41. A downstream gold job should run only after `main.silver.orders` is updated. Which trigger should you use?

    **Sample answer:** Use a table update trigger on `main.silver.orders`.

42. A gold job needs both `main.silver.orders` and `main.silver.customers` to be updated before it runs. What trigger configuration fits?

    **Sample answer:** Use a table update trigger that monitors both tables and is configured to run when all required tables have updated.

43. A bronze ingestion job updates `main.bronze.orders_raw`, and a silver job starts from that table update. What trigger style is this?

    **Sample answer:** Data-driven orchestration using a table update trigger.

44. A job must run every hour even if no table changes occur, because it checks freshness and sends SLA alerts. Which trigger fits?

    **Sample answer:** Use a scheduled trigger.

45. A dashboard must be ready by 8 AM every weekday. Which trigger is usually safer: scheduled or data-driven?

    **Sample answer:** Scheduled is usually safer for a wall-clock SLA, often with freshness checks and alerts for missing or late data.

46. Files arrive 0 to many times per day, and empty job runs are expensive. Which trigger style is better?

    **Sample answer:** Data-driven, specifically a file arrival trigger if the readiness signal is new files.

47. An upstream SaaS ingestion finishes at variable times and writes a bronze Delta table. A silver job should not guess the finish time. Which trigger fits?

    **Sample answer:** Use a table update trigger on the bronze table.

48. What is the main downside of scheduling silver at 1 AM if bronze sometimes finishes at 1:30 AM?

    **Sample answer:** Silver may run on stale or incomplete data. A table update trigger or an explicit dependency would better represent readiness.

49. What is the main downside of using only data-driven triggers for an executive report that must publish at a fixed time?

    **Sample answer:** If data arrives late, the report may publish late. A scheduled job with monitoring may better satisfy the fixed-time SLA.

50. A single job contains ingest, transform, and dashboard refresh tasks. Should you create separate schedules between each task?

    **Sample answer:** No. Use one trigger for the job and DAG dependencies between tasks.

51. When should you prefer one multi-task job over multiple separately scheduled jobs?

    **Sample answer:** When the tasks are tightly coupled, owned together, and form one logical workflow with clear dependencies.

52. When should you prefer separate jobs connected by table update triggers?

    **Sample answer:** When pipelines are independently owned or the table itself is the contract between teams or layers.

53. A scheduled job's trigger is paused. What happens to new automatic runs?

    **Sample answer:** New automatic runs do not start while the trigger is paused. Active runs can continue.

54. A job is scheduled every minute but each run takes five minutes, and max concurrent runs is one. What can happen?

    **Sample answer:** New scheduled runs can be skipped or delayed because the previous run is still active and concurrency is limited.

55. Why should scheduled jobs use UTC when possible?

    **Sample answer:** UTC avoids daylight saving time surprises and makes schedules easier to reason about across regions.

56. A job scheduled in a daylight saving time zone runs hourly. What issue can occur during DST changes?

    **Sample answer:** An hourly run can be skipped or delayed when the local clock shifts.

57. A task should fail the workflow if a validation query finds bad data. Which pattern fits?

    **Sample answer:** Use a validation task that raises an error or returns a failing result, then let downstream tasks depend on its success.

58. A validation failure should skip downstream transforms but still send a notification. Which features help?

    **Sample answer:** Use dependencies or an If/else branch to skip transforms, and configure a notification task with failure-oriented `Run if` behavior.

59. A task should run if no upstream dependency failed, even if one branch was skipped. Which `Run if` option is often appropriate?

    **Sample answer:** Use `Run if = None failed`, assuming skipped or excluded branches are acceptable for the workflow.

60. In one sentence, what is the most important Domain 4 rule?

    **Sample answer:** Use Lakeflow Jobs to express orchestration explicitly: choose the right task type, connect tasks with DAG dependencies, use control-flow features for branching/retries/loops, and pick scheduled or data-driven triggers based on the real readiness signal.
