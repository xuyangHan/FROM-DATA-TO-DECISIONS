# Data Engineering

This area collects data engineering learning notes, practical guidance, and
certification study material.

Data engineering concepts are often easy to explain in isolation. The hard
part is applying them when data is late, requirements change, systems fail,
costs grow, and several reasonable solutions have different tradeoffs. The
series below cover both sides: building strong fundamentals and making sound
decisions in real systems.

## Blog Series

### Fundamentals to Advanced

Fundamentals to Advanced is a roadmap-style series for learning data
engineering in a deliberate order. It starts with first principles and
gradually moves toward designing, operating, and improving production data
platforms.

Use this series when you want to understand a topic, see how it connects to the
larger field, or decide what to learn next.

The planned learning path covers:

1. What data engineers do and how data moves through an organization
2. SQL, Python, data formats, storage, and compute fundamentals
3. Data modelling, warehouses, lakes, and lakehouses
4. Batch processing, streaming, and orchestration
5. Testing, data quality, observability, and reliability
6. Security, governance, performance, and cost
7. Architecture, platform design, and advanced tradeoffs

Articles will be listed here in recommended reading order. Each article should
explain why the topic matters, introduce the core concepts, show a practical
example, identify common mistakes, and point toward the next topic.

| Article | What you will learn |
|---------|---------------------|
| [What Is Data Engineering?](fundamentals-to-advanced/00-what-is-DE.md) | What data engineering is, why it is needed, and how data moves from its source to its users |

### Industry Experience

Industry Experience is a problem-driven series about the gap between data
engineering concepts and their behavior in real production environments. Each
article starts with a practical issue, examines why apparently simple solutions
can fail, and works through the tradeoffs behind a robust solution.

Use this series when you want to develop production judgment and learn from
problems that are rarely visible in tutorials.

Topics may include:

- A pipeline succeeds but produces incomplete or incorrect data
- Backfills interfere with daily processing
- Source schemas and business definitions change unexpectedly
- Duplicate, late, or out-of-order records create inconsistent results
- Small pipelines become slow or expensive as data volume grows
- Ownership, alerts, and runbooks fail during incidents
- A technically correct design becomes difficult for a team to operate

Articles will be listed here as standalone cases and can later be grouped into
themes such as reliability, performance, data quality, or team practices. Each
article should compare viable approaches and finish with practical lessons or a
reusable decision framework.

| Article | Practical question |
|---------|--------------------|
| [Why CI/CD Feels Different in Data Engineering](industry-experience/01-CICD-in-DE.md) | How can ADF and Terraform changes be tested and promoted without making production the first real test? |
| [Why Can My Laptop Connect but the Pipeline Cannot?](industry-experience/02-laptop-connects-pipeline-cannot.md) | How do you diagnose the real network path and request only the firewall access a production runtime needs? |
| [The Private Endpoint Exists, So Why Does Connectivity Still Fail?](industry-experience/03-private-endpoint-connectivity.md) | Why do private endpoints still fail when DNS, routing, or endpoint approval is wrong? |
| [Why Does ADF Need a Machine in the Middle?](industry-experience/04-adf-machine-in-the-middle.md) | Which component moves data into private networks, and how should SHIR be operated? |
| [The SFTP Feed Broke After Key Rotation](industry-experience/05-sftp-key-rotation.md) | How do SSH identity, encryption, compatibility, and safe key rotation fit together? |
| [Encryption Worked, but Trust Failed](industry-experience/06-tls-trust-failed.md) | Why can an encrypted TLS connection still fail certificate validation or client authentication? |
| [Why Should Nobody Know the Production Password?](industry-experience/07-passwordless-production-access.md) | How can workloads access production systems without spreading long-lived credentials? |
| [Yesterday's Data Was Loaded Twice](industry-experience/08-yesterdays-data-loaded-twice.md) | How do idempotency, control state, and atomic publication make retries safe? |
| [The Daily Full Load No Longer Finishes Before Morning](industry-experience/09-full-load-no-longer-finishes.md) | When should a pipeline move from full loads to watermarks, CDC, or streaming? |
| [The Raw File Arrived, but Is It Ready for Analytics?](industry-experience/10-raw-file-ready-for-analytics.md) | How should raw inputs, schema changes, formats, and curated data layers be managed? |
| [The Pipeline Is Slow Only During Peak Hours](industry-experience/11-peak-hour-pipeline-performance.md) | How do you isolate a production bottleneck before adding more compute or concurrency? |

## Databricks Series

### Databricks Certified Data Engineer Associate

PySpark-first study notes and practice material mapped to the Associate exam
domains. SQL snippets are labelled where the exam favors SQL. This is the best
starting point for readers new to Databricks or preparing for the Associate
certification.

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
| 1.1 Core components, Delta Lake, Unity Catalog | [1.1-core-components.md](Databricks%20DE%20Associate/01-platform/1.1-core-components.md) |
| 1.2 Compute services and cost | [1.2-compute-services.md](Databricks%20DE%20Associate/01-platform/1.2-compute-services.md) |

##### Domain 2 - Data Ingestion and Loading

| Subdomain | File |
|-----------|------|
| 2.1 Ingestion patterns: batch, streaming, incremental | [2.1-ingestion-patterns.md](Databricks%20DE%20Associate/02-ingestion/2.1-ingestion-patterns.md) |
| 2.2 COPY INTO | [2.2-copy-into.md](Databricks%20DE%20Associate/02-ingestion/2.2-copy-into.md) |
| 2.3 Auto Loader | [2.3-auto-loader.md](Databricks%20DE%20Associate/02-ingestion/2.3-auto-loader.md) |
| 2.4 Lakeflow Connect | [2.4-lakeflow-connect.md](Databricks%20DE%20Associate/02-ingestion/2.4-lakeflow-connect.md) |
| 2.5 JDBC, ODBC, REST, and Jobs | [2.5-jdbc-rest-jobs.md](Databricks%20DE%20Associate/02-ingestion/2.5-jdbc-rest-jobs.md) |
| 2.6 Ingestion method prioritization | [2.6-ingestion-decision-matrix.md](Databricks%20DE%20Associate/02-ingestion/2.6-ingestion-decision-matrix.md) |
| 2.7 Semi-structured and unstructured data | [2.7-semi-structured-data.md](Databricks%20DE%20Associate/02-ingestion/2.7-semi-structured-data.md) |

##### Domain 3 - Data Transformation and Modeling

| Subdomain | File |
|-----------|------|
| 3.1 Bronze to Silver cleaning | [3.1-bronze-to-silver.md](Databricks%20DE%20Associate/03-transformation/3.1-bronze-to-silver.md) |
| 3.2 Joins and unions | [3.2-joins-unions.md](Databricks%20DE%20Associate/03-transformation/3.2-joins-unions.md) |
| 3.3 Column and row manipulation | [3.3-column-row-ops.md](Databricks%20DE%20Associate/03-transformation/3.3-column-row-ops.md) |
| 3.4 Deduplication and aggregates | [3.4-dedup-aggregates.md](Databricks%20DE%20Associate/03-transformation/3.4-dedup-aggregates.md) |
| 3.5 Spark tuning | [3.5-spark-tuning.md](Databricks%20DE%20Associate/03-transformation/3.5-spark-tuning.md) |
| 3.6 Gold layer objects | [3.6-gold-layer-objects.md](Databricks%20DE%20Associate/03-transformation/3.6-gold-layer-objects.md) |
| 3.7 Data quality | [3.7-data-quality.md](Databricks%20DE%20Associate/03-transformation/3.7-data-quality.md) |

##### Domain 4 - Lakeflow Jobs

| Subdomain | File |
|-----------|------|
| 4.1 Control flow: retries and branching | [4.1-control-flow.md](Databricks%20DE%20Associate/04-jobs/4.1-control-flow.md) |
| 4.2 Task types and DAGs | [4.2-tasks-dependencies.md](Databricks%20DE%20Associate/04-jobs/4.2-tasks-dependencies.md) |
| 4.3 Trigger types | [4.3-triggers.md](Databricks%20DE%20Associate/04-jobs/4.3-triggers.md) |
| 4.4 Time vs data-driven triggers | [4.4-trigger-selection.md](Databricks%20DE%20Associate/04-jobs/4.4-trigger-selection.md) |

##### Domain 5 - CI/CD

| Subdomain | File |
|-----------|------|
| 5.1 Repos and Git in workspace | [5.1-repos-git.md](Databricks%20DE%20Associate/05-cicd/5.1-repos-git.md) |
| 5.2 Bundle variables and overrides | [5.2-bundle-variables.md](Databricks%20DE%20Associate/05-cicd/5.2-bundle-variables.md) |
| 5.3 Bundle deploy and promotion | [5.3-bundle-deploy.md](Databricks%20DE%20Associate/05-cicd/5.3-bundle-deploy.md) |
| 5.4 Databricks CLI | [5.4-databricks-cli.md](Databricks%20DE%20Associate/05-cicd/5.4-databricks-cli.md) |

##### Domain 6 - Troubleshooting, Monitoring, Optimization

| Subdomain | File |
|-----------|------|
| 6.1 Job run history trends | [6.1-job-run-history.md](Databricks%20DE%20Associate/06-troubleshooting/6.1-job-run-history.md) |
| 6.2 Jobs UI health monitoring | [6.2-jobs-ui-monitoring.md](Databricks%20DE%20Associate/06-troubleshooting/6.2-jobs-ui-monitoring.md) |
| 6.3 Spark UI bottlenecks | [6.3-spark-ui-bottlenecks.md](Databricks%20DE%20Associate/06-troubleshooting/6.3-spark-ui-bottlenecks.md) |
| 6.4 Liquid clustering and predictive optimization | [6.4-liquid-clustering.md](Databricks%20DE%20Associate/06-troubleshooting/6.4-liquid-clustering.md) |
| 6.5 Cluster failures and OOM | [6.5-cluster-diagnostics.md](Databricks%20DE%20Associate/06-troubleshooting/6.5-cluster-diagnostics.md) |

##### Domain 7 - Governance and Security

| Subdomain | File |
|-----------|------|
| 7.1 Managed vs external tables | [7.1-managed-external-tables.md](Databricks%20DE%20Associate/07-governance/7.1-managed-external-tables.md) |
| 7.2 GRANT, REVOKE, and DENY | [7.2-access-controls.md](Databricks%20DE%20Associate/07-governance/7.2-access-controls.md) |
| 7.3 Column masks and row filters | [7.3-masking-rls.md](Databricks%20DE%20Associate/07-governance/7.3-masking-rls.md) |
| 7.4 ABAC policies | [7.4-abac-policies.md](Databricks%20DE%20Associate/07-governance/7.4-abac-policies.md) |

#### Practice

- [Platform practice](Databricks%20DE%20Associate/practice/01-platform.md)
- [Ingestion practice](Databricks%20DE%20Associate/practice/02-ingestion.md)
- [Transformation practice](Databricks%20DE%20Associate/practice/03-transformation.md)
- [Jobs practice](Databricks%20DE%20Associate/practice/04-jobs.md)
- [CI/CD practice](Databricks%20DE%20Associate/practice/05-cicd.md)
- [Troubleshooting practice](Databricks%20DE%20Associate/practice/06-troubleshooting.md)
- [Governance practice](Databricks%20DE%20Associate/practice/07-governance.md)

#### Conventions

- One file represents one exam subdomain.
- Use `catalog.schema.table` for governed Unity Catalog reads and writes.
- Medallion architecture is introduced in domains 2.1 and 3.1.

### Databricks Certified Data Engineer Professional

PySpark-first study notes mapped to the Professional exam domains. This series
builds on the Associate material and emphasizes production tradeoffs involving
reliability, cost, performance, governance, observability, and deployment.

Complete the Associate notes first, then work through the Professional domains
on development, ingestion, transformation, cost and performance, and data
modelling before the remaining domains.

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
| Lakeflow Jobs | Orchestration: multi-task DAGs, schedules, triggers, and notifications |
| Lakeflow Connect | Managed and standard connectors for enterprise sources |
| Lakeflow Spark Declarative Pipelines | Formerly Delta Live Tables (DLT): declarative ETL |
| Databricks Asset Bundles (DAB) | `databricks.yml` CI/CD packaging and deployment |
| Delta Sharing | Secure live data sharing across Databricks and external platforms |
| Lakehouse Federation | Governed querying across supported external source systems |

#### Domain Map

| Domain | Topic | Files |
|--------|-------|-------|
| 1 | Developing code with Python and SQL | [Python tools](Databricks%20DE%20Professional/1.1-python-tools-development.md), [building and testing ETL](Databricks%20DE%20Professional/1.2-building-testing-etl.md) |
| 2 | Data ingestion and acquisition | [Data ingestion and acquisition](Databricks%20DE%20Professional/2.1-data-ingestion-acquisition.md) |
| 3 | Data transformation, cleansing, and quality | [Transformation, cleansing, and quality](Databricks%20DE%20Professional/3.1-transformation-cleansing-quality.md) |
| 4 | Data sharing and federation | [Data sharing and federation](Databricks%20DE%20Professional/4.1-data-sharing-federation.md) |
| 5 | Monitoring and alerting | [Monitoring](Databricks%20DE%20Professional/5.1-monitoring.md), [alerting](Databricks%20DE%20Professional/5.2-alerting.md) |
| 6 | Cost and performance optimisation | [Cost and performance optimisation](Databricks%20DE%20Professional/6.1-cost-performance-optimisation.md) |
| 7 | Data security and compliance | [Data security mechanisms](Databricks%20DE%20Professional/7.1-data-security-mechanisms.md), [ensuring compliance](Databricks%20DE%20Professional/7.2-ensuring-compliance.md) |
| 8 | Data governance | [Data governance](Databricks%20DE%20Professional/8.1-data-governance.md) |
| 9 | Debugging and deploying | [Debugging and troubleshooting](Databricks%20DE%20Professional/9.1-debugging-troubleshooting.md), [deploying CI/CD](Databricks%20DE%20Professional/9.2-deploying-cicd.md) |
| 10 | Data modelling | [Data modelling](Databricks%20DE%20Professional/10.1-data-modelling.md) |

#### Conventions

- One file represents one official exam subdomain.
- Use `catalog.schema.table` for governed Unity Catalog reads and writes.
- Treat Medallion architecture as assumed knowledge and refer to the Associate notes where needed.
- Emphasize production tradeoffs: reliability, cost, performance, governance, observability, and deployment.

## How the Series Fit Together

| Series | Primary goal | Organization |
|--------|--------------|--------------|
| Fundamentals to Advanced | Build broad data engineering knowledge | Learning roadmap |
| Industry Experience | Develop practical engineering judgment | Real-world problems |
| Databricks DE Associate | Learn Databricks foundations and prepare for the exam | Certification domains |
| Databricks DE Professional | Study advanced Databricks production practices | Certification domains |

The roadmap and industry series are platform-neutral where possible. The
Databricks series provide platform-specific depth and can be read alongside
related topics in either blog series.
