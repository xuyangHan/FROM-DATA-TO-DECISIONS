# Practice answers

Links point to cheat sheets for review.

---

## Domain 1 — Platform

| # | Answer | Review |
|---|--------|--------|
| 1 | **Job cluster** (ephemeral, per run) | [1.2](../01-platform/1.2-compute-services.md) |
| 2 | **SQL warehouse** | [1.2](../01-platform/1.2-compute-services.md) |
| 3 | **Delta Lake** | [1.1](../01-platform/1.1-core-components.md) |
| 4 | **Unity Catalog** (`catalog.schema.table`) | [1.1](../01-platform/1.1-core-components.md) |
| 5 | Day: **all-purpose** or **serverless**; night: **job cluster** | [1.2](../01-platform/1.2-compute-services.md) |
| 6 | Cloud storage in the **metastore’s managed location** (UC-controlled) | [7.1](../07-governance/7.1-managed-external-tables.md) |

---

## Domain 2 — Ingestion

| # | Answer | Review |
|---|--------|--------|
| 1 | **Auto Loader** (`cloudFiles`) | [2.3](../02-ingestion/2.3-auto-loader.md), [2.6](../02-ingestion/2.6-ingestion-decision-matrix.md) |
| 2 | **COPY INTO** (incremental batch SQL) | [2.2](../02-ingestion/2.2-copy-into.md) |
| 3 | **Lakeflow Connect** (managed connector) | [2.4](../02-ingestion/2.4-lakeflow-connect.md) |
| 4 | **Notebook** (REST → DataFrame) + **Lakeflow Job** on schedule; secrets for token | [2.5](../02-ingestion/2.5-jdbc-rest-jobs.md) |
| 5 | Auto Loader scales file tracking; COPY INTO simpler but poor at huge file counts | [2.6](../02-ingestion/2.6-ingestion-decision-matrix.md) |
| 6 | `mergeSchema` / **schema evolution**, `rescuedDataColumn`, `cloudFiles.schemaLocation` | [2.3](../02-ingestion/2.3-auto-loader.md) |
| 7 | `partitionColumn`, `lowerBound`, `upperBound`, `numPartitions` | [2.5](../02-ingestion/2.5-jdbc-rest-jobs.md) |
| 8 | **`explode`** (or explode_outer) on tags in silver | [2.7](../02-ingestion/2.7-semi-structured-data.md), [3.3](../03-transformation/3.3-column-row-ops.md) |
| 9 | Target is **UC-governed Delta table** (`saveAsTable` / registered table) | [2.6](../02-ingestion/2.6-ingestion-decision-matrix.md) |
| 10 | When **no** first-party Connect connector and requirement explicitly names partner | [2.6](../02-ingestion/2.6-ingestion-decision-matrix.md) |

---

## Domain 3 — Transformation

| # | Answer | Review |
|---|--------|--------|
| 1 | `read` bronze → clean → `write` **new** silver table (`saveAsTable`); do not overwrite bronze | [3.1](../03-transformation/3.1-bronze-to-silver.md) |
| 2 | **`broadcast(dim)`** or auto broadcast threshold | [3.2](../03-transformation/3.2-joins-unions.md) |
| 3 | **Left** join | [3.2](../03-transformation/3.2-joins-unions.md) |
| 4 | **`unionByName`** with `allowMissingColumns=True` | [3.2](../03-transformation/3.2-joins-unions.md) |
| 5 | **`dropDuplicates(["order_id"])`** | [3.4](../03-transformation/3.4-dedup-aggregates.md) |
| 6 | **`approx_count_distinct`** | [3.4](../03-transformation/3.4-dedup-aggregates.md) |
| 7 | **Data skew**; salt key, broadcast, or filter skewed keys | [6.3](../06-troubleshooting/6.3-spark-ui-bottlenecks.md), [3.5](../03-transformation/3.5-spark-tuning.md) |
| 8 | **Materialized view** (or gold Delta table); refresh hourly | [3.6](../03-transformation/3.6-gold-layer-objects.md) |

---

## Domain 4 — Jobs

| # | Answer | Review |
|---|--------|--------|
| 1 | **Dependencies** + **condition task** (if/else on A result) | [4.1](../04-jobs/4.1-control-flow.md) |
| 2 | **SQL task** on **SQL warehouse** | [4.2](../04-jobs/4.2-tasks-dependencies.md) |
| 3 | SQL task **depends on** upstream; **dashboard task** depends on SQL | [4.2](../04-jobs/4.2-tasks-dependencies.md) |
| 4 | **Table update** trigger | [4.3](../04-jobs/4.3-triggers.md), [4.4](../04-jobs/4.4-trigger-selection.md) |
| 5 | **Scheduled (cron)** trigger | [4.3](../04-jobs/4.3-triggers.md) |
| 6 | **max_retries** + **min_retry_interval_millis** | [4.1](../04-jobs/4.1-control-flow.md) |

---

## Domain 5 — CI/CD

| # | Answer | Review |
|---|--------|--------|
| 1 | Feature **branch** → commit/push → **pull request** | [5.1](../05-cicd/5.1-repos-git.md) |
| 2 | **Bundle variables** per **target** (`dev` / `prod`) | [5.2](../05-cicd/5.2-bundle-variables.md) |
| 3 | **`databricks.yml`** + `resources/*.yml` | [5.3](../05-cicd/5.3-bundle-deploy.md) |
| 4 | **`databricks bundle validate`** | [5.4](../05-cicd/5.4-databricks-cli.md) |
| 5 | **`run_as`** (service principal) on target/job | [5.3](../05-cicd/5.3-bundle-deploy.md) |

---

## Domain 6 — Troubleshooting

| # | Answer | Review |
|---|--------|--------|
| 1 | **Lakeflow Jobs run history** — compare durations | [6.1](../06-troubleshooting/6.1-job-run-history.md) |
| 2 | Fix **`transform`** (failed upstream); publish skipped as consequence | [6.2](../06-troubleshooting/6.2-jobs-ui-monitoring.md) |
| 3 | Adjust **`spark.sql.shuffle.partitions`**; fix skew/broadcast; increase executor memory | [3.5](../03-transformation/3.5-spark-tuning.md), [6.3](../06-troubleshooting/6.3-spark-ui-bottlenecks.md) |
| 4 | **Liquid clustering** on `country`, `date` — not high-cardinality `user_id` partition | [6.4](../06-troubleshooting/6.4-liquid-clustering.md) |
| 5 | **Aggregate/write** to table; avoid driver `collect()` | [6.5](../06-troubleshooting/6.5-cluster-diagnostics.md) |
| 6 | Cluster **event log** | [6.5](../06-troubleshooting/6.5-cluster-diagnostics.md) |

---

## Domain 7 — Governance

| # | Answer | Review |
|---|--------|--------|
| 1 | **Files remain** in `LOCATION`; only metadata dropped | [7.1](../07-governance/7.1-managed-external-tables.md) |
| 2 | Table-level **`SELECT`** (plus `USE SCHEMA` / `USE CATALOG`) | [7.2](../07-governance/7.2-access-controls.md) |
| 3 | **Column mask** | [7.3](../07-governance/7.3-masking-rls.md) |
| 4 | **Row filter** (or ABAC row policy) | [7.3](../07-governance/7.3-masking-rls.md) |
| 5 | **ABAC policy** on `pii` tag | [7.4](../07-governance/7.4-abac-policies.md) |
| 6 | **`USE CATALOG`**, **`USE SCHEMA`**, **`MODIFY`** (or table-level MODIFY) on silver for SP | [7.2](../07-governance/7.2-access-controls.md) |
