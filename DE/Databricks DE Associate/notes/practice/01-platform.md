# Domain 1 - Platform (practice)

Use these to test whether you can choose the right platform component or compute type from a short exam-style scenario.

## Questions and sample answers

1. A team runs nightly ETL that must not share an interactive cluster with analysts. Which compute should the Lakeflow Job use?

   **Sample answer:** Use serverless jobs compute when the task is supported, or classic jobs compute if the workload needs custom Spark settings or unsupported features. Do not use an all-purpose cluster for this production job.

2. BI analysts run `SELECT` dashboards only, with no PySpark. What compute type fits?

   **Sample answer:** Use a SQL warehouse, preferably a serverless SQL warehouse when available.

3. You need ACID merges and time travel on curated tables. Which storage format is required?

   **Sample answer:** Delta Lake. Delta tables provide ACID transactions, `MERGE`, transaction history, and time travel.

4. Where do you register `main.sales.orders` so lineage and `GRANT` apply?

   **Sample answer:** Register it in Unity Catalog as a governed object using the three-level namespace `catalog.schema.table`.

5. A notebook exploratory session runs during the day, and a production job runs at night. Name the best compute choice for each.

   **Sample answer:** Use serverless notebook compute or classic all-purpose compute for daytime exploration. Use serverless jobs compute or classic jobs compute for the nightly production job.

6. Managed Delta table data for Unity Catalog is best described as stored where?

   **Sample answer:** In a Unity Catalog-managed storage location in cloud object storage. UC manages both the metadata and the lifecycle of the underlying files.

7. A company has raw files in object storage but wants warehouse-like reliability, SQL access, governance, and ETL on the same data. Is this a data lake or a lakehouse pattern?

   **Sample answer:** Lakehouse. A lakehouse adds Delta table reliability, governance through Unity Catalog, and compute access patterns on top of cloud object storage.

8. Which Databricks layer is responsible for notebooks, jobs UI, cluster management, and workspace administration: control plane or data plane?

   **Sample answer:** Control plane. The data plane is where compute runs and data is processed or stored in the customer's cloud environment.

9. A team needs to store PDFs and JSON drops with Unity Catalog governance, but they are not tables. What should they use?

   **Sample answer:** Use a Unity Catalog volume. Volumes govern non-tabular files.

10. A table has old versions that are needed for debugging a bad load from yesterday. Which Delta feature helps?

    **Sample answer:** Time travel. Query the table using `VERSION AS OF` or `TIMESTAMP AS OF`.

11. A pipeline must apply upserts from a change feed into a curated customer table. Which Delta operation is the expected answer?

    **Sample answer:** Use `MERGE INTO` to update matching rows and insert new rows.

12. A user wants to manually delete files inside a Delta table directory to save storage. What is the exam-safe response?

    **Sample answer:** Do not manually edit Delta data files or `_delta_log`. Use Delta table operations and `VACUUM` for cleanup.

13. What is the top-level container for Unity Catalog metadata and governance in a region?

    **Sample answer:** A Unity Catalog metastore.

14. In `main.silver.orders`, identify the catalog, schema, and table.

    **Sample answer:** `main` is the catalog, `silver` is the schema, and `orders` is the table.

15. A team has existing data in a cloud bucket that is also written by tools outside Databricks. Should they usually choose a managed table or an external table?

    **Sample answer:** External table. Unity Catalog governs access, but the team keeps responsibility for the external storage path and lifecycle.

16. A team creates a new Databricks-first curated table and wants the simplest lifecycle management. Should they choose a managed table or an external table?

    **Sample answer:** Managed table. Unity Catalog manages metadata and the underlying data files.

17. Which UI is most useful for browsing catalogs, schemas, table permissions, owners, tags, and lineage?

    **Sample answer:** Catalog Explorer.

18. A developer needs R language support and RDD APIs in an interactive notebook. Should they use serverless notebook compute or classic all-purpose compute?

    **Sample answer:** Classic all-purpose compute. Serverless notebooks do not support R and do not support RDD APIs.

19. A job task is a JAR or Spark submit task. Which compute is the safe choice?

    **Sample answer:** Classic jobs compute. Serverless jobs do not support every task type.

20. A streaming notebook uses a processing-time trigger and must run continuously. Is serverless notebook/jobs compute the best fit?

    **Sample answer:** No. Serverless notebook/jobs streaming supports limited triggers; `Trigger.AvailableNow()` is the recommended serverless pattern. Use supported pipeline mode or classic jobs compute for unsupported continuous patterns.

21. A SQL dashboard has unpredictable bursts of user queries and needs fast startup. Which SQL warehouse type is usually recommended?

    **Sample answer:** Serverless SQL warehouse.

22. A cluster is expensive because it stays idle for hours after development work. What setting should be enabled?

    **Sample answer:** Auto-termination. Autoscaling can help with worker count, but auto-termination stops idle compute.

23. A manager says, "Double the workers; the job will definitely be faster and cheaper." What should you say?

    **Sample answer:** More workers can help only if the workload can use the parallelism. Check shuffle, skew, spills, file sizes, and runtime before scaling, because cost is roughly rate times time.

24. Is Photon a separate compute type like a SQL warehouse or job cluster?

    **Sample answer:** No. Photon is an execution engine that accelerates supported SQL, DataFrame, ETL, and streaming workloads. It is enabled across multiple compute types.
