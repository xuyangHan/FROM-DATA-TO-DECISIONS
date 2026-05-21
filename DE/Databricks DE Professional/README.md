# Databricks Certified Data Engineer Professional - Study Notes

Cheat sheets mapped 1:1 to [outline.md](outline.md), which follows the official exam guide. **PySpark-first**; SQL snippets labeled where the exam favors SQL.

## Exam snapshot

| Item | Detail |
|------|--------|
| Questions | 59 scored, multiple choice |
| Time | 120 minutes |
| Delivery | Online proctored or test center |
| Code on exam | SQL and Python |
| Test aides | None, including API documentation |
| Validity | 2 years |
| Guide | [November 30, 2025 exam guide](https://www.databricks.com/sites/default/files/2025-11/databricks-certified-data-engineer-professional-exam-guide-november-30-2025_0.pdf) |

## Glossary (Lakeflow naming)

| Term | Meaning |
|------|---------|
| **Lakeflow Jobs** | Orchestration (multi-task DAG, schedules, triggers, notifications) |
| **Lakeflow Connect** | Managed/standard connectors for enterprise sources |
| **Lakeflow Spark Declarative Pipelines** | Formerly Delta Live Tables (DLT) - declarative ETL |
| **Databricks Asset Bundles (DAB)** | `databricks.yml` CI/CD packaging and deployment |
| **Delta Sharing** | Secure live data sharing across Databricks and external platforms |
| **Lakehouse Federation** | Governed querying across supported external source systems |

## Domain map

### Domain 1 - Developing Code for Data Processing using Python and SQL

| Subdomain | File |
|-----------|------|
| 1.1 Using Python and Tools for development | [1.1-python-tools-development.md](01-code/1.1-python-tools-development.md) |
| 1.2 Building and Testing an ETL pipeline with Lakeflow Spark Declarative Pipelines, SQL, and Apache Spark on the Databricks Platform | [1.2-building-testing-etl.md](01-code/1.2-building-testing-etl.md) |

### Domain 2 - Data Ingestion & Acquisition

| Subdomain | File |
|-----------|------|
| 2.1 Data Ingestion & Acquisition | [2.1-data-ingestion-acquisition.md](02-ingestion/2.1-data-ingestion-acquisition.md) |

### Domain 3 - Data Transformation, Cleansing, and Quality

| Subdomain | File |
|-----------|------|
| 3.1 Data Transformation, Cleansing, and Quality | [3.1-transformation-cleansing-quality.md](03-transformation-quality/3.1-transformation-cleansing-quality.md) |

### Domain 4 - Data Sharing and Federation

| Subdomain | File |
|-----------|------|
| 4.1 Data Sharing and Federation | [4.1-data-sharing-federation.md](04-sharing-federation/4.1-data-sharing-federation.md) |

### Domain 5 - Monitoring and Alerting

| Subdomain | File |
|-----------|------|
| 5.1 Monitoring | [5.1-monitoring.md](05-monitoring-alerting/5.1-monitoring.md) |
| 5.2 Alerting | [5.2-alerting.md](05-monitoring-alerting/5.2-alerting.md) |

### Domain 6 - Cost & Performance Optimisation

| Subdomain | File |
|-----------|------|
| 6.1 Cost & Performance Optimisation | [6.1-cost-performance-optimisation.md](06-cost-performance/6.1-cost-performance-optimisation.md) |

### Domain 7 - Ensuring Data Security and Compliance

| Subdomain | File |
|-----------|------|
| 7.1 Applying Data Security mechanisms | [7.1-data-security-mechanisms.md](07-security-compliance/7.1-data-security-mechanisms.md) |
| 7.2 Ensuring Compliance | [7.2-ensuring-compliance.md](07-security-compliance/7.2-ensuring-compliance.md) |

### Domain 8 - Data Governance

| Subdomain | File |
|-----------|------|
| 8.1 Data Governance | [8.1-data-governance.md](08-governance/8.1-data-governance.md) |

### Domain 9 - Debugging and Deploying

| Subdomain | File |
|-----------|------|
| 9.1 Debugging and Troubleshooting | [9.1-debugging-troubleshooting.md](09-debugging-deploying/9.1-debugging-troubleshooting.md) |
| 9.2 Deploying CI/CD | [9.2-deploying-cicd.md](09-debugging-deploying/9.2-deploying-cicd.md) |

### Domain 10 - Data Modelling

| Subdomain | File |
|-----------|------|
| 10.1 Data Modelling | [10.1-data-modelling.md](10-data-modelling/10.1-data-modelling.md) |

## Practice (phase 2)

Scenario questions: [practice/README.md](practice/README.md) - answers in [practice/answers.md](practice/answers.md).

## Conventions

- One file = one official exam subdomain (~1-2 pages).
- Copy [_template.md](_template.md) when adding topics.
- **Unity Catalog** path: `catalog.schema.table` on all governed reads/writes.
- **Medallion** architecture is treated as assumed knowledge; link back to Associate notes where needed.
- Professional notes should emphasize production tradeoffs: reliability, cost, performance, governance, observability, and deployment.
