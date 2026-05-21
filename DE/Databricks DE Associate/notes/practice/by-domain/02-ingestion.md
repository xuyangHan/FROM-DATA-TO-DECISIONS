# Domain 2 — Ingestion (practice)

1. 50,000 small JSON files arrive daily under `s3://logs/incoming/`. Governance requires UC table `main.bronze.logs`. Best ingestion tool?

2. A CSV dump lands once per week in ADLS (hundreds of files). SQL team owns the pipeline. Simplest incremental approach?

3. Salesforce data must sync to bronze with minimal custom code. Connector exists in Lakeflow Connect managed. Your choice?

4. One-off REST API with 2 MB JSON response, hourly schedule. Connect not available. Pattern?

5. Compare: Auto Loader vs COPY INTO for millions of files per day.

6. Schema adds new optional fields in JSON over time. Which Auto Loader options matter?

7. PostgreSQL table 500 GB, partitioned on `id`. Notebook extract nightly. Two JDBC options to parallelize read?

8. Semi-structured SaaS events with nested `payload` — land bronze via Connect, then what PySpark op for tags array?

9. You need Unity Catalog lineage on landing tables. What must be true of the target?

10. Partner marketplace connector vs Lakeflow Connect — when is partner acceptable in exam logic?
