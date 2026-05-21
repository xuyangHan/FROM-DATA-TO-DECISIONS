# Databricks Certified Data Engineer Associate — Study Notes

Cheat sheets mapped 1:1 to [outline.md](../outline.md). **PySpark-first**; SQL snippets labeled where the exam favors SQL.

## Exam snapshot

| Item | Detail |
|------|--------|
| Questions | 45 scored, multiple choice |
| Time | 90 minutes |
| Delivery | Online proctored or test center |
| Code on exam | SQL when possible; otherwise Python |
| Validity | 2 years |
| Guide | [May 2026 exam guide](https://www.databricks.com/sites/default/files/2026-05/databricks-certified-data-engineer-associate-exam-guide-may-2026.pdf) |

## Glossary (Lakeflow naming)

| Term | Meaning |
|------|---------|
| **Lakeflow Jobs** | Orchestration (multi-task DAG, schedules, triggers) |
| **Lakeflow Connect** | Managed/standard connectors for enterprise sources |
| **Lakeflow Spark Declarative Pipelines** | Formerly Delta Live Tables (DLT) — declarative ETL |
| **Automation Bundle** | Formerly Databricks Asset Bundles (DAB) — `databricks.yml` CI/CD |

## Recommended study order

1. [02-ingestion/2.6-ingestion-decision-matrix.md](02-ingestion/2.6-ingestion-decision-matrix.md) — unlocks “pick the right tool” questions
2. [01-platform/1.2-compute-services.md](01-platform/1.2-compute-services.md)
3. [03-transformation/](03-transformation/) — 3.2–3.5 (joins, tuning)
4. [04-jobs/](04-jobs/)
5. Remaining ingestion + transformation (3.6–3.7)
6. [05-cicd/](05-cicd/) → [06-troubleshooting/](06-troubleshooting/) → [07-governance/](07-governance/)

## Domain map

### Domain 1 — Databricks Intelligence Platform

| Subdomain | File |
|-----------|------|
| 1.1 Core components, Delta Lake, Unity Catalog | [1.1-core-components.md](01-platform/1.1-core-components.md) |
| 1.2 Compute services & cost | [1.2-compute-services.md](01-platform/1.2-compute-services.md) |

### Domain 2 — Data Ingestion and Loading

| Subdomain | File |
|-----------|------|
| 2.1 Ingestion patterns (batch/streaming/incremental) | [2.1-ingestion-patterns.md](02-ingestion/2.1-ingestion-patterns.md) |
| 2.2 COPY INTO | [2.2-copy-into.md](02-ingestion/2.2-copy-into.md) |
| 2.3 Auto Loader | [2.3-auto-loader.md](02-ingestion/2.3-auto-loader.md) |
| 2.4 Lakeflow Connect | [2.4-lakeflow-connect.md](02-ingestion/2.4-lakeflow-connect.md) |
| 2.5 JDBC/ODBC/REST + Jobs | [2.5-jdbc-rest-jobs.md](02-ingestion/2.5-jdbc-rest-jobs.md) |
| 2.6 Ingestion method prioritization | [2.6-ingestion-decision-matrix.md](02-ingestion/2.6-ingestion-decision-matrix.md) |
| 2.7 Semi-structured / unstructured | [2.7-semi-structured-data.md](02-ingestion/2.7-semi-structured-data.md) |

### Domain 3 — Data Transformation and Modeling

| Subdomain | File |
|-----------|------|
| 3.1 Bronze → Silver cleaning | [3.1-bronze-to-silver.md](03-transformation/3.1-bronze-to-silver.md) |
| 3.2 Joins & unions | [3.2-joins-unions.md](03-transformation/3.2-joins-unions.md) |
| 3.3 Column/row manipulation | [3.3-column-row-ops.md](03-transformation/3.3-column-row-ops.md) |
| 3.4 Dedup & aggregates | [3.4-dedup-aggregates.md](03-transformation/3.4-dedup-aggregates.md) |
| 3.5 Spark tuning | [3.5-spark-tuning.md](03-transformation/3.5-spark-tuning.md) |
| 3.6 Gold layer objects | [3.6-gold-layer-objects.md](03-transformation/3.6-gold-layer-objects.md) |
| 3.7 Data quality | [3.7-data-quality.md](03-transformation/3.7-data-quality.md) |

### Domain 4 — Lakeflow Jobs

| Subdomain | File |
|-----------|------|
| 4.1 Control flow (retries, branching) | [4.1-control-flow.md](04-jobs/4.1-control-flow.md) |
| 4.2 Task types & DAG | [4.2-tasks-dependencies.md](04-jobs/4.2-tasks-dependencies.md) |
| 4.3 Trigger types | [4.3-triggers.md](04-jobs/4.3-triggers.md) |
| 4.4 Time vs data-driven triggers | [4.4-trigger-selection.md](04-jobs/4.4-trigger-selection.md) |

### Domain 5 — CI/CD

| Subdomain | File |
|-----------|------|
| 5.1 Repos & Git in workspace | [5.1-repos-git.md](05-cicd/5.1-repos-git.md) |
| 5.2 Bundle variables & overrides | [5.2-bundle-variables.md](05-cicd/5.2-bundle-variables.md) |
| 5.3 Bundle deploy & promotion | [5.3-bundle-deploy.md](05-cicd/5.3-bundle-deploy.md) |
| 5.4 Databricks CLI | [5.4-databricks-cli.md](05-cicd/5.4-databricks-cli.md) |

### Domain 6 — Troubleshooting, Monitoring, Optimization

| Subdomain | File |
|-----------|------|
| 6.1 Job run history trends | [6.1-job-run-history.md](06-troubleshooting/6.1-job-run-history.md) |
| 6.2 Jobs UI health monitoring | [6.2-jobs-ui-monitoring.md](06-troubleshooting/6.2-jobs-ui-monitoring.md) |
| 6.3 Spark UI bottlenecks | [6.3-spark-ui-bottlenecks.md](06-troubleshooting/6.3-spark-ui-bottlenecks.md) |
| 6.4 Liquid clustering & predictive optimization | [6.4-liquid-clustering.md](06-troubleshooting/6.4-liquid-clustering.md) |
| 6.5 Cluster failures & OOM | [6.5-cluster-diagnostics.md](06-troubleshooting/6.5-cluster-diagnostics.md) |

### Domain 7 — Governance and Security

| Subdomain | File |
|-----------|------|
| 7.1 Managed vs external tables | [7.1-managed-external-tables.md](07-governance/7.1-managed-external-tables.md) |
| 7.2 GRANT / REVOKE / DENY | [7.2-access-controls.md](07-governance/7.2-access-controls.md) |
| 7.3 Column masks & row filters | [7.3-masking-rls.md](07-governance/7.3-masking-rls.md) |
| 7.4 ABAC policies | [7.4-abac-policies.md](07-governance/7.4-abac-policies.md) |

## Practice (phase 2)

Scenario questions: [practice/README.md](practice/README.md) — answers in [practice/answers.md](practice/answers.md).

## Conventions

- One file = one exam subdomain (~1–2 pages).
- Copy [_template.md](_template.md) when adding topics.
- **Unity Catalog** path: `catalog.schema.table` on all governed reads/writes.
- **Medallion** defined in 2.1 and 3.1; other notes link back.
