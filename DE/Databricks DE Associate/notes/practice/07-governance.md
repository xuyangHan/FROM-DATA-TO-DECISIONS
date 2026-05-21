# Domain 7 — Governance (practice)

1. `DROP TABLE` on external table — what happens to S3 files?

2. Analysts need `SELECT` on `main.gold.revenue` but cannot see other gold tables. Grant level?

3. Contractors must never see `salary` column though they have `SELECT` on table. Mechanism?

4. EU analysts see only EU rows in shared table. Mechanism?

5. 500 tables tagged `pii=true` need same email mask. Per-table mask or ABAC?

6. Job service principal needs to write `main.silver.*`. Minimum privileges?
