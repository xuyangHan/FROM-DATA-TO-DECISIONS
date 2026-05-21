Domain 1: Databricks Intelligence Platform
Subdomain 1.1: Understand the core components of the Databricks Data Intelligence Platform, such as its architecture, Delta Lake, and Unity Catalog.

Understand the core components of the Databricks Data Intelligence Platform, such as its architecture, Delta Lake, and Unity Catalog.

Subdomain 1.2: Understand Databricks Data Intelligence Platform’s compute services, including their characteristics, limitations, and cost models, and select the most suitable option for each workload use case.

Understand Databricks Data Intelligence Platform’s compute services, including their characteristics, limitations, and cost models, and select the most suitable option for each workload use case.

Domain 2: Data Ingestion and Loading
Subdomain 2.1: Enable and detail data ingestion patterns, including batch, streaming, and incremental loading, and import data from sources such as local ﬁles, Lakeﬂow Connect standard connectors, and Lakeﬂow Connect managed connectors.

Enable and detail data ingestion patterns, including batch, streaming, and incremental loading, and import data from sources such as local ﬁles, Lakeﬂow Connect standard connectors, and Lakeﬂow Connect managed connectors.

Subdomain 2.2: Use the COPY INTO command to incrementally load ﬁles from cloud object storage (ADLS/S3/GCS) into Unity‑Catalog–governed tables.

Use the COPY INTO command to incrementally load ﬁles from cloud object storage (ADLS/S3/GCS) into Unity‑Catalog–governed tables.

Subdomain 2.3: Use Auto Loader with schema enforcement and schema evolution in batch modes (for example, directory listing or ﬁle notiﬁcation) to land data into Unity‑Catalog–governed tables.

Use Auto Loader with schema enforcement and schema evolution in batch modes (for example, directory listing or ﬁle notiﬁcation) to land data into Unity‑Catalog–governed tables.

Subdomain 2.4: Conﬁgure Lakeﬂow Connect to reliably ingest data from diverse enterprise sources into Unity‑Catalog–governed tables.

Conﬁgure Lakeﬂow Connect to reliably ingest data from diverse enterprise sources into Unity‑Catalog–governed tables.

Subdomain 2.5: Use JDBC/ODBC or REST clients in notebooks to land data into cloud storage or directly into Unity‑Catalog–governed tables, usually orchestrated and scheduled with Lakeﬂow Jobs.

Use JDBC/ODBC or REST clients in notebooks to land data into cloud storage or directly into Unity‑Catalog–governed tables, usually orchestrated and scheduled with Lakeﬂow Jobs.

Subdomain 2.6: Prioritize between Auto Loader, Lakeﬂow Connect (standard and managed connectors), partner connectors, and other ingestion methods based on technical requirements such as data volume, ingestion frequency, data types, and governance needs with Unity Catalog.

Prioritize between Auto Loader, Lakeﬂow Connect (standard and managed connectors), partner connectors, and other ingestion methods based on technical requirements such as data volume, ingestion frequency, data types, and governance needs with Unity Catalog.

Subdomain 2.7: Ingest semi-structured and unstructured data (for example, JSON and nested data) via Lakeﬂow Connect and other managed connectors into Unity‑Catalog–governed Delta tables.

Ingest semi-structured and unstructured data (for example, JSON and nested data) via Lakeﬂow Connect and other managed connectors into Unity‑Catalog–governed Delta tables.

Domain 3: Data Transformation and Modeling
Subdomain 3.1: Implement data cleaning by reading bronze tables with PySpark/SQL, cleaning nulls, standardizing data types, and writing to new silver tables.

Implement data cleaning by reading bronze tables with PySpark/SQL, cleaning nulls, standardizing data types, and writing to new silver tables.

Subdomain 3.2: Combine DataFrames with operations such as Inner join, left join, broadcast join, multiple keys, cross join, union, and union all.

Combine DataFrames with operations such as Inner join, left join, broadcast join, multiple keys, cross join, union, and union all.

Subdomain 3.3: Manipulate columns, rows, and table structures by adding, dropping, splitting, renaming column names, applying ﬁlters, and exploding arrays.

Manipulate columns, rows, and table structures by adding, dropping, splitting, renaming column names, applying ﬁlters, and exploding arrays.

Subdomain 3.4: Perform data deduplication operations and aggregate operations on DataFrames, such as count, approximate count distinct, and mean, summary.

Perform data deduplication operations and aggregate operations on DataFrames, such as count, approximate count distinct, and mean, summary.

Subdomain 3.5: Understand the basic tuning parameters (spark.sql.shufﬂe.partitions:, spark.default.parallelism, spark.executor/driver.memory, spark.sql.autoBroadcastJoinThreshold) and re-measure the performance.

Understand the basic tuning parameters (spark.sql.shufﬂe.partitions:, spark.default.parallelism, spark.executor/driver.memory, spark.sql.autoBroadcastJoinThreshold) and re-measure the performance.

Subdomain 3.6: Understand the difference between, and how to build, Gold layer objects such as materialized views, views, streaming tables, and tables for BI and analytics teams in Unity Catalog.

Understand the difference between, and how to build, Gold layer objects such as materialized views, views, streaming tables, and tables for BI and analytics teams in Unity Catalog.

Subdomain 3.7: Apply data quality checks and validation rules to ensure reliable Silver and Gold datasets.

Apply data quality checks and validation rules to ensure reliable Silver and Gold datasets.

Domain 4: Working with Lakeﬂow Jobs
Subdomain 4.1: Implement control ﬂows (retries and conditional tasks such as branching and looping) using Lakeﬂow Jobs for pipeline orchestration

Implement control ﬂows (retries and conditional tasks such as branching and looping) using Lakeﬂow Jobs for pipeline orchestration

Subdomain 4.2: Conﬁgure common tasks (notebook, SQL query, dashboard, and pipeline tasks) and their dependencies using Lakeﬂow Jobs and its DAG‑based task graph

Conﬁgure common tasks (notebook, SQL query, dashboard, and pipeline tasks) and their dependencies using Lakeﬂow Jobs and its DAG‑based task graph

Subdomain 4.3: Implement job schedules using Lakeﬂow Jobs with an understanding of trigger types (scheduled, ﬁle arrival, and table update)

Implement job schedules using Lakeﬂow Jobs with an understanding of trigger types (scheduled, ﬁle arrival, and table update)

Subdomain 4.4: Choose between time‑based and data‑driven triggers based on data availability and pipeline dependencies.

Choose between time‑based and data‑driven triggers based on data availability and pipeline dependencies.

Domain 5: Implementing CI/CD
Subdomain 5.1: Manage your code development workﬂow within the Databricks workspace UI, including creating and switching between branches in Databricks Repos, committing and pushing changes, and creating pull requests using Databricks Git integration.

Manage your code development workﬂow within the Databricks workspace UI, including creating and switching between branches in Databricks Repos, committing and pushing changes, and creating pull requests using Databricks Git integration.

Subdomain 5.2: Understand environment-speciﬁc conﬁguration using Automation Bundle (formerly Databricks Asset Bundles) variables and overrides while promoting the same codebase across dev, test, and prod targets.

Understand environment-speciﬁc conﬁguration using Automation Bundle (formerly Databricks Asset Bundles) variables and overrides while promoting the same codebase across dev, test, and prod targets.

Subdomain 5.3: Deploy Declarative Automation Bundles (formerly Databricks Asset Bundles) to package, conﬁgure, and promote Lakeﬂow Jobs, Lakeﬂow Spark Declarative Pipelines, and other workspace assets across dev, test, and prod environments.

Deploy Declarative Automation Bundles (formerly Databricks Asset Bundles) to package, conﬁgure, and promote Lakeﬂow Jobs, Lakeﬂow Spark Declarative Pipelines, and other workspace assets across dev, test, and prod environments.

Subdomain 5.4: Understand the Databricks CLI to validate, deploy, and manage Declarative Automation Bundles (formerly Databricks Asset Bundles) and other workspace assets in automated CI/CD workﬂows.

Understand the Databricks CLI to validate, deploy, and manage Declarative Automation Bundles (formerly Databricks Asset Bundles) and other workspace assets in automated CI/CD workﬂows.

Domain 6: Troubleshooting, Monitoring, and Optimization
Subdomain 6.1: Identify trends in job performance using the Lakeﬂow Jobs run history view to compare current execution times against historical baselines.

Identify trends in job performance using the Lakeﬂow Jobs run history view to compare current execution times against historical baselines.

Subdomain 6.2: Use the Lakeﬂow Jobs UI to monitor pipeline health by interpreting job statuses, viewing DAG‑based task graphs to spot upstream blockers, and tracking pipeline run times and failure rates.

Use the Lakeﬂow Jobs UI to monitor pipeline health by interpreting job statuses, viewing DAG‑based task graphs to spot upstream blockers, and tracking pipeline run times and failure rates.

Subdomain 6.3: Identify common performance bottlenecks such as data skew, shufﬂing, and disk spilling by interpreting stage-level metrics in the Spark UI.

Identify common performance bottlenecks such as data skew, shufﬂing, and disk spilling by interpreting stage-level metrics in the Spark UI.

Subdomain 6.4: Understand the features of Liquid Clustering and predictive optimization.

Understand the features of Liquid Clustering and predictive optimization.

Subdomain 6.5: Diagnose cluster startup failures, library conﬂicts, and out-of-memory issues.

Diagnose cluster startup failures, library conﬂicts, and out-of-memory issues.

Domain 7: Governance and Security
Subdomain 7.1: Differentiate between managed and external tables in Unity Catalog and perform basic operations (create, modify, delete, and convert between managed and external tables) on them.

Differentiate between managed and external tables in Unity Catalog and perform basic operations (create, modify, delete, and convert between managed and external tables) on them.

Subdomain 7.2: Conﬁgure access controls using the UI and SQL by applying GRANT, REVOKE, and DENY privileges to principals (users, groups, and service principals) at appropriate levels of the security hierarchy.

Conﬁgure access controls using the UI and SQL by applying GRANT, REVOKE, and DENY privileges to principals (users, groups, and service principals) at appropriate levels of the security hierarchy.

Subdomain 7.3: Understand column-level masking and row-level security to restrict data visibility based on user groups.

Understand column-level masking and row-level security to restrict data visibility based on user groups.

Subdomain 7.4: Understand Unity Catalog ABAC policies to centrally control row-level ﬁltering and column masking for sensitive data.

Understand Unity Catalog ABAC policies to centrally control row-level ﬁltering and column masking for sensitive data.