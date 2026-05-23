# From Data to Decisions

A practical learning and reference repo for data work, from business intelligence and analysis to data engineering, Databricks, governance, and future data topics.

This repo is:

- A learning resource for aspiring data analysts, BI analysts, and data engineers
- A reference for core data concepts, tools, and workflows
- A portfolio-style collection of applied notes, projects, and study guides
- A growing knowledge base for data-related topics beyond BI

---

## Repository Map

| Area | What it covers |
|------|----------------|
| [BI](BI/) | Business intelligence, SQL analysis, metrics, Python analysis, visualization, data modeling, warehousing, experimentation, and interviews |
| [DE](DE/) | Data engineering study notes for the Databricks Certified Data Engineer Associate and Professional exams |
| [projects](projects/) | Applied analysis notebooks and writeups |

---

## Business Intelligence and Data Analysis

### Foundations

- [What is Business Intelligence?](BI/blogs/01-what-is-bi.md)

### SQL and Metrics

- [SQL for Analysis](BI/blogs/02-SQL-for-analysis.md)
- [Building Reliable Metrics](BI/blogs/03-Building-Reliable-Metrics.md)
- [Real BI Analysis in SQL](BI/blogs/04-Real-BI-Analysis-in-SQL.md)

### Python and Analysis Workflow

- [Python for BI](BI/blogs/05-Python-for-BI.md)
- [Data Wrangling in Python](BI/blogs/06-Data-Wrangling-in-Python.md)
- [Exploratory Data Analysis](BI/blogs/07-Exploratory-Data-Analysis.md)
- [Python Data Visualization](BI/blogs/08-Python-Data-Visualization.md)

### Data Architecture

- [Data Modeling for BI](BI/blogs/09-Data-Modeling-for-BI.md)
- [Data Warehousing](BI/blogs/10-Data-Warehousing.md)

### Experimentation

- [A/B Testing for BI](BI/blogs/11-AB-Testing.md)

### Interviews

- [BI, Data Analyst, and Data Engineer Interview Questions](BI/blogs/12-BI-Data-Interview-Questions.md)
- [Data Engineer Interview Questions](BI/blogs/13-Data-Engineer-Interview-Questions.md)

### Tools

- Excel for quick BI analysis
- Tableau dashboards for BI
- Power BI
- Python notebooks for repeatable analysis

---

## Data Engineering

### Databricks Certified Data Engineer Associate

Study notes for the Databricks Certified Data Engineer Associate exam. The notes are PySpark-first, with SQL snippets labeled where the exam favors SQL.

#### Exam Snapshot

| Item | Detail |
|------|--------|
| Questions | 45 scored, multiple choice |
| Time | 90 minutes |
| Delivery | Online proctored or test center |
| Code on exam | SQL when possible; otherwise Python |
| Validity | 2 years |
| Guide | [May 2026 exam guide](https://www.databricks.com/sites/default/files/2026-05/databricks-certified-data-engineer-associate-exam-guide-may-2026.pdf) |

#### Lakeflow Naming Glossary

| Term | Meaning |
|------|---------|
| Lakeflow Jobs | Orchestration: multi-task DAGs, schedules, and triggers |
| Lakeflow Connect | Managed and standard connectors for enterprise sources |
| Lakeflow Spark Declarative Pipelines | Formerly Delta Live Tables (DLT): declarative ETL |
| Automation Bundle | Formerly Databricks Asset Bundles (DAB): `databricks.yml` CI/CD |

#### Domain Map

##### Domain 1 - Databricks Intelligence Platform

| Subdomain | File |
|-----------|------|
| 1.1 Core components, Delta Lake, Unity Catalog | [1.1-core-components.md](DE/Databricks%20DE%20Associate/01-platform/1.1-core-components.md) |
| 1.2 Compute services and cost | [1.2-compute-services.md](DE/Databricks%20DE%20Associate/01-platform/1.2-compute-services.md) |

##### Domain 2 - Data Ingestion and Loading

| Subdomain | File |
|-----------|------|
| 2.1 Ingestion patterns: batch, streaming, incremental | [2.1-ingestion-patterns.md](DE/Databricks%20DE%20Associate/02-ingestion/2.1-ingestion-patterns.md) |
| 2.2 COPY INTO | [2.2-copy-into.md](DE/Databricks%20DE%20Associate/02-ingestion/2.2-copy-into.md) |
| 2.3 Auto Loader | [2.3-auto-loader.md](DE/Databricks%20DE%20Associate/02-ingestion/2.3-auto-loader.md) |
| 2.4 Lakeflow Connect | [2.4-lakeflow-connect.md](DE/Databricks%20DE%20Associate/02-ingestion/2.4-lakeflow-connect.md) |
| 2.5 JDBC, ODBC, REST, and Jobs | [2.5-jdbc-rest-jobs.md](DE/Databricks%20DE%20Associate/02-ingestion/2.5-jdbc-rest-jobs.md) |
| 2.6 Ingestion method prioritization | [2.6-ingestion-decision-matrix.md](DE/Databricks%20DE%20Associate/02-ingestion/2.6-ingestion-decision-matrix.md) |
| 2.7 Semi-structured and unstructured data | [2.7-semi-structured-data.md](DE/Databricks%20DE%20Associate/02-ingestion/2.7-semi-structured-data.md) |

##### Domain 3 - Data Transformation and Modeling

| Subdomain | File |
|-----------|------|
| 3.1 Bronze to Silver cleaning | [3.1-bronze-to-silver.md](DE/Databricks%20DE%20Associate/03-transformation/3.1-bronze-to-silver.md) |
| 3.2 Joins and unions | [3.2-joins-unions.md](DE/Databricks%20DE%20Associate/03-transformation/3.2-joins-unions.md) |
| 3.3 Column and row manipulation | [3.3-column-row-ops.md](DE/Databricks%20DE%20Associate/03-transformation/3.3-column-row-ops.md) |
| 3.4 Deduplication and aggregates | [3.4-dedup-aggregates.md](DE/Databricks%20DE%20Associate/03-transformation/3.4-dedup-aggregates.md) |
| 3.5 Spark tuning | [3.5-spark-tuning.md](DE/Databricks%20DE%20Associate/03-transformation/3.5-spark-tuning.md) |
| 3.6 Gold layer objects | [3.6-gold-layer-objects.md](DE/Databricks%20DE%20Associate/03-transformation/3.6-gold-layer-objects.md) |
| 3.7 Data quality | [3.7-data-quality.md](DE/Databricks%20DE%20Associate/03-transformation/3.7-data-quality.md) |

##### Domain 4 - Lakeflow Jobs

| Subdomain | File |
|-----------|------|
| 4.1 Control flow: retries and branching | [4.1-control-flow.md](DE/Databricks%20DE%20Associate/04-jobs/4.1-control-flow.md) |
| 4.2 Task types and DAGs | [4.2-tasks-dependencies.md](DE/Databricks%20DE%20Associate/04-jobs/4.2-tasks-dependencies.md) |
| 4.3 Trigger types | [4.3-triggers.md](DE/Databricks%20DE%20Associate/04-jobs/4.3-triggers.md) |
| 4.4 Time vs data-driven triggers | [4.4-trigger-selection.md](DE/Databricks%20DE%20Associate/04-jobs/4.4-trigger-selection.md) |

##### Domain 5 - CI/CD

| Subdomain | File |
|-----------|------|
| 5.1 Repos and Git in workspace | [5.1-repos-git.md](DE/Databricks%20DE%20Associate/05-cicd/5.1-repos-git.md) |
| 5.2 Bundle variables and overrides | [5.2-bundle-variables.md](DE/Databricks%20DE%20Associate/05-cicd/5.2-bundle-variables.md) |
| 5.3 Bundle deploy and promotion | [5.3-bundle-deploy.md](DE/Databricks%20DE%20Associate/05-cicd/5.3-bundle-deploy.md) |
| 5.4 Databricks CLI | [5.4-databricks-cli.md](DE/Databricks%20DE%20Associate/05-cicd/5.4-databricks-cli.md) |

##### Domain 6 - Troubleshooting, Monitoring, Optimization

| Subdomain | File |
|-----------|------|
| 6.1 Job run history trends | [6.1-job-run-history.md](DE/Databricks%20DE%20Associate/06-troubleshooting/6.1-job-run-history.md) |
| 6.2 Jobs UI health monitoring | [6.2-jobs-ui-monitoring.md](DE/Databricks%20DE%20Associate/06-troubleshooting/6.2-jobs-ui-monitoring.md) |
| 6.3 Spark UI bottlenecks | [6.3-spark-ui-bottlenecks.md](DE/Databricks%20DE%20Associate/06-troubleshooting/6.3-spark-ui-bottlenecks.md) |
| 6.4 Liquid clustering and predictive optimization | [6.4-liquid-clustering.md](DE/Databricks%20DE%20Associate/06-troubleshooting/6.4-liquid-clustering.md) |
| 6.5 Cluster failures and OOM | [6.5-cluster-diagnostics.md](DE/Databricks%20DE%20Associate/06-troubleshooting/6.5-cluster-diagnostics.md) |

##### Domain 7 - Governance and Security

| Subdomain | File |
|-----------|------|
| 7.1 Managed vs external tables | [7.1-managed-external-tables.md](DE/Databricks%20DE%20Associate/07-governance/7.1-managed-external-tables.md) |
| 7.2 GRANT, REVOKE, and DENY | [7.2-access-controls.md](DE/Databricks%20DE%20Associate/07-governance/7.2-access-controls.md) |
| 7.3 Column masks and row filters | [7.3-masking-rls.md](DE/Databricks%20DE%20Associate/07-governance/7.3-masking-rls.md) |
| 7.4 ABAC policies | [7.4-abac-policies.md](DE/Databricks%20DE%20Associate/07-governance/7.4-abac-policies.md) |

#### Practice

- [Platform practice](DE/Databricks%20DE%20Associate/practice/01-platform.md)
- [Ingestion practice](DE/Databricks%20DE%20Associate/practice/02-ingestion.md)
- [Transformation practice](DE/Databricks%20DE%20Associate/practice/03-transformation.md)
- [Jobs practice](DE/Databricks%20DE%20Associate/practice/04-jobs.md)
- [CI/CD practice](DE/Databricks%20DE%20Associate/practice/05-cicd.md)
- [Troubleshooting practice](DE/Databricks%20DE%20Associate/practice/06-troubleshooting.md)
- [Governance practice](DE/Databricks%20DE%20Associate/practice/07-governance.md)

Full Associate index: [Databricks DE Associate README](DE/Databricks%20DE%20Associate/README.md)

### Databricks Certified Data Engineer Professional

Study notes for the Databricks Certified Data Engineer Professional exam. Cheat sheets map 1:1 to the [Professional domain map](DE/Databricks%20DE%20Professional/README.md#domain-map). Notes are **PySpark-first**, with SQL snippets labeled where the exam favors SQL. Professional content builds on the Associate track and emphasizes production tradeoffs: reliability, cost, performance, governance, observability, and deployment.

#### Exam Snapshot

| Item | Detail |
|------|--------|
| Questions | 59 scored, multiple choice |
| Time | 120 minutes |
| Delivery | Online proctored or test center |
| Code on exam | SQL and Python |
| Test aides | None, including API documentation |
| Validity | 2 years |
| Guide | [November 30, 2025 exam guide](https://www.databricks.com/sites/default/files/2025-11/databricks-certified-data-engineer-professional-exam-guide-november-30-2025_0.pdf) |

#### Lakeflow Naming Glossary

| Term | Meaning |
|------|---------|
| Lakeflow Jobs | Orchestration: multi-task DAGs, schedules, triggers, notifications |
| Lakeflow Connect | Managed and standard connectors for enterprise sources |
| Lakeflow Spark Declarative Pipelines | Formerly Delta Live Tables (DLT): declarative ETL |
| Databricks Asset Bundles (DAB) | `databricks.yml` CI/CD packaging and deployment |
| Delta Sharing | Secure live data sharing across Databricks and external platforms |
| Lakehouse Federation | Governed querying across supported external source systems |

#### Domain Map

##### Domain 1 - Developing Code for Data Processing using Python and SQL

| Subdomain | File |
|-----------|------|
| 1.1 Using Python and tools for development | [1.1-python-tools-development.md](DE/Databricks%20DE%20Professional/1.1-python-tools-development.md) |
| 1.2 Building and testing ETL with Lakeflow SDP, SQL, and Spark | [1.2-building-testing-etl.md](DE/Databricks%20DE%20Professional/1.2-building-testing-etl.md) |

##### Domain 2 - Data Ingestion and Acquisition

| Subdomain | File |
|-----------|------|
| 2.1 Data ingestion and acquisition | [2.1-data-ingestion-acquisition.md](DE/Databricks%20DE%20Professional/2.1-data-ingestion-acquisition.md) |

##### Domain 3 - Data Transformation, Cleansing, and Quality

| Subdomain | File |
|-----------|------|
| 3.1 Transformation, cleansing, and quality | [3.1-transformation-cleansing-quality.md](DE/Databricks%20DE%20Professional/3.1-transformation-cleansing-quality.md) |

##### Domain 4 - Data Sharing and Federation

| Subdomain | File |
|-----------|------|
| 4.1 Data sharing and federation | [4.1-data-sharing-federation.md](DE/Databricks%20DE%20Professional/4.1-data-sharing-federation.md) |

##### Domain 5 - Monitoring and Alerting

| Subdomain | File |
|-----------|------|
| 5.1 Monitoring | [5.1-monitoring.md](DE/Databricks%20DE%20Professional/5.1-monitoring.md) |
| 5.2 Alerting | [5.2-alerting.md](DE/Databricks%20DE%20Professional/5.2-alerting.md) |

##### Domain 6 - Cost and Performance Optimisation

| Subdomain | File |
|-----------|------|
| 6.1 Cost and performance optimisation | [6.1-cost-performance-optimisation.md](DE/Databricks%20DE%20Professional/6.1-cost-performance-optimisation.md) |

##### Domain 7 - Ensuring Data Security and Compliance

| Subdomain | File |
|-----------|------|
| 7.1 Data security mechanisms | [7.1-data-security-mechanisms.md](DE/Databricks%20DE%20Professional/7.1-data-security-mechanisms.md) |
| 7.2 Ensuring compliance | [7.2-ensuring-compliance.md](DE/Databricks%20DE%20Professional/7.2-ensuring-compliance.md) |

##### Domain 8 - Data Governance

| Subdomain | File |
|-----------|------|
| 8.1 Data governance | [8.1-data-governance.md](DE/Databricks%20DE%20Professional/8.1-data-governance.md) |

##### Domain 9 - Debugging and Deploying

| Subdomain | File |
|-----------|------|
| 9.1 Debugging and troubleshooting | [9.1-debugging-troubleshooting.md](DE/Databricks%20DE%20Professional/9.1-debugging-troubleshooting.md) |
| 9.2 Deploying CI/CD | [9.2-deploying-cicd.md](DE/Databricks%20DE%20Professional/9.2-deploying-cicd.md) |

##### Domain 10 - Data Modelling

| Subdomain | File |
|-----------|------|
| 10.1 Data modelling | [10.1-data-modelling.md](DE/Databricks%20DE%20Professional/10.1-data-modelling.md) |

#### Practice

Practice scenario questions are planned for a future phase. See [Professional README](DE/Databricks%20DE%20Professional/README.md) for conventions.

**Suggested study path:** Complete the Associate notes first, then work through Professional domains 1-3, 6, and 10, then 4-5 and 7-9.

---

## Projects

- [Early COVID Data Analysis](projects/early-covid-data-analysis.md)
- [Python Visualization Notebook](projects/python-vis.ipynb)

---

## Who This Is For

- Aspiring data analysts and BI analysts
- Aspiring data engineers
- Developers transitioning into data roles
- Anyone who wants to understand how data becomes decisions, pipelines, models, metrics, and products

## Future Topics

This repo can expand into:

- Analytics engineering
- dbt and semantic layers
- Cloud data platforms
- Data governance and quality
- Machine learning foundations
- Dashboard and data product design
- Interview preparation across data roles

## Star or Fork

If you find this useful, feel free to star or fork the repo for your own learning journey.
