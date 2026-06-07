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

## Databricks Series

### Databricks Certified Data Engineer Associate

[Databricks DE Associate](Databricks%20DE%20Associate/) contains PySpark-first
study notes and practice material mapped to the Associate exam domains. Topics
include the Databricks platform, ingestion, transformation, Lakeflow Jobs,
CI/CD, troubleshooting, optimization, governance, and security.

This is the best starting point for readers new to Databricks or preparing for
the Associate certification.

### Databricks Certified Data Engineer Professional

[Databricks DE Professional](Databricks%20DE%20Professional/) contains
PySpark-first study notes mapped to the Professional exam domains. It builds on
the Associate material and emphasizes production tradeoffs involving
reliability, cost, performance, governance, observability, and deployment.

Complete the Associate notes first, then work through the Professional domains
on development, ingestion, transformation, cost and performance, and data
modelling before the remaining domains.

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
