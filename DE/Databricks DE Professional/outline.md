Domain 1: Developing Code for Data Processing using Python and SQL
Subdomain 1.1: Using Python and Tools for development

- Design and implement a scalable Python project structure optimized for Databricks Asset Bundles (DABs), enabling modular development, deployment automation, and CI/CD integration.
- Manage and troubleshoot external third-party library installations and dependencies in Databricks, including PyPI packages, local wheels, and source archives.
- Develop User-Deﬁned Functions (UDFs) using Pandas/Python UDF.

Subdomain 1.2: Building and Testing an ETL pipeline with Lakeﬂow Spark Declarative Pipelines, SQL, and Apache Spark on the Databricks Platform

- Build and manage reliable, production-ready data pipelines for batch and streaming data using Lakeﬂow Spark Declarative Pipelines and Autoloader.
- Create and Automate ETL workloads using Jobs via UI/APIs/CLI.
- Explain the advantages and disadvantages of streaming tables compared to materialized views.
- Use APPLY CHANGES APIs to simplify CDC in Lakeﬂow Spark Declarative Pipelines.
- Compare Spark Structured Streaming and Lakeﬂow Spark Declarative Pipelines to determine the optimal approach for building scalable ETL pipelines.
- Create a pipeline component that uses control ﬂow operators (e.g., if/else, for/each, etc.).
- Choose the appropriate conﬁgs for environments and dependencies, high memory for notebook tasks, and auto-optimization to disallow retries.
- Develop unit and integration tests using assertDataFrameEqual, assertSchemaEqual, DataFrame.transform, and testing frameworks, to ensure code correctness, including a built-in debugger.

Domain 2: Data Ingestion & Acquisition
Subdomain 2.1: Data Ingestion & Acquisition

- Design and implement data ingestion pipelines to efﬁciently ingest a variety of data formats including Delta Lake, Parquet, ORC, AVRO, JSON, CSV, XML, Text and Binary from diverse sources such as message buses and cloud storage.
- Create an append-only data pipeline capable of handling both batch and streaming data using Delta.

Domain 3: Data Transformation, Cleansing, and Quality
Subdomain 3.1: Data Transformation, Cleansing, and Quality

- Write efﬁcient Spark SQL and PySpark code to apply advanced data transformations, including window functions, joins, and aggregations, to manipulate and analyze large Datasets.
- Develop a quarantining process for bad data with Lakeﬂow Spark Declarative Pipelines, or autoloader in classic jobs.

Domain 4: Data Sharing and Federation
Subdomain 4.1: Data Sharing and Federation

- Demonstrate delta sharing securely between Databricks deployments using Databricks to Databricks Sharing (D2D) or to external platforms using the open sharing protocol (D2O).
- Conﬁgure Lakehouse Federation with proper governance across the supported source Systems.
- Use Delta Share to share live data from Lakehouse to any computing platform.

Domain 5: Monitoring and Alerting
Subdomain 5.1: Monitoring

- Use system tables for observability over resource utilization, cost, auditing and workload monitoring.
- Use Query Proﬁler UI and Spark UI to monitor workloads.
- Use the Databricks REST APIs/Databricks CLI for monitoring jobs and pipelines.
- Use Lakeﬂow Spark Declarative Pipelines Event Logs to monitor pipelines.

Subdomain 5.2: Alerting

- Use SQL Alerts to monitor data quality.
- Use the Lakeﬂow Jobs UI and Jobs API to set up notiﬁcations for job status and performance issues.

Domain 6: Cost & Performance Optimisation
Subdomain 6.1: Cost & Performance Optimisation

- Understand how / why using Unity Catalog managed tables reduces operations overhead and maintenance burden.
- Understand delta optimization techniques, such as deletion vectors and liquid clustering.
- Understand the optimization techniques used by Databricks to ensure the performance of queries on large datasets (data skipping, ﬁle pruning, etc.).
- Apply Change Data Feed (CDF) to address speciﬁc limitations of streaming tables and enhance latency.
- Use the query proﬁle to analyze the query and identify bottlenecks, such as bad data skipping, inefﬁcient types of joins, and data shufﬂing.

Domain 7: Ensuring Data Security and Compliance
Subdomain 7.1: Applying Data Security mechanisms

- Use ACLs to secure Workspace Objects, enforcing the principle of least privilege, including enforcing principles like least privilege, policy enforcement.
- Use row ﬁlters and column masks to ﬁlter and mask sensitive table data.
- Apply anonymization and pseudonymization methods, such as Hashing, Tokenization, Suppression, and generalisation, to conﬁdential data.

Subdomain 7.2: Ensuring Compliance

- Implement a compliant batch & streaming pipeline that detects and applies masking of PII to ensure data privacy.
- Develop a data purging solution ensuring compliance with data retention policies.

Domain 8: Data Governance
Subdomain 8.1: Data Governance

- Create and add descriptions/metadata about enterprise data to make it more discoverable.
- Demonstrate understanding of Unity Catalog permission inheritance model.

Domain 9: Debugging and Deploying
Subdomain 9.1: Debugging and Troubleshooting

- Identify pertinent diagnostic information using Spark UI, cluster logs, system tables, and query proﬁles to troubleshoot errors.
- Analyze the errors and remediate the failed job runs with job repairs and parameter overrides.
- Use Lakeﬂow Spark Declarative Pipelines event logs and the Spark UI to debug Lakeﬂow Spark Declarative Pipelines and Spark pipelines.

Subdomain 9.2: Deploying CI/CD

- Build and deploy Databricks resources using Databricks Asset Bundles.
- Conﬁgure and integrate with Git-based CI/CD workﬂows using Databricks Git Folders for notebook and code deployment.

Domain 10: Data Modelling
Subdomain 10.1: Data Modelling

- Design and implement scalable data models using Delta Lake to manage large datasets.
- Simplify data layout decisions and optimize query performance using Liquid Clustering.
- Identify the beneﬁts of using liquid Clustering over Partitioning and ZOrder.
- Design Dimensional Models for analytical workloads, ensuring efﬁcient querying and aggregation.