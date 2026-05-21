# Domain 7 - Governance (practice)

## Quick questions

1. `DROP TABLE` on an external table - what happens to the S3/ADLS/GCS files?

2. Analysts need `SELECT` on `main.gold.revenue` but cannot see other gold tables. What is the minimum grant pattern?

3. Contractors must never see the `salary` column though they can query the employee table. What mechanism should you use?

4. EU analysts should see only EU rows in a shared table. What mechanism should you use?

5. 500 tables tagged `pii=true` need the same email mask. Per-table mask or ABAC?

6. A Lakeflow Jobs service principal needs to write existing tables in `main.silver`. What minimum privileges are likely needed?

7. A user has `SELECT` on `main.gold.revenue` but gets a permission error saying they do not have `USE CATALOG` on `main`. What is missing?

8. The exam scenario says "Use `DENY` on a Unity Catalog table to block contractors." What should you remember?

9. A column mask UDF for a `STRING` column returns `INT`. What is wrong?

10. A row filter UDF returns `STRING` values such as `'Y'` and `'N'`. What is wrong?

11. You want users to discover tables and request access but not read data. Which privilege helps?

12. You converted an external Delta table to managed and need to verify it. Which command helps?

## Answers

1. Only Unity Catalog table metadata is dropped. The external files remain and must be deleted separately if required.

2. Grant `USE CATALOG` on `main`, `USE SCHEMA` on `main.gold`, and `SELECT` on `TABLE main.gold.revenue` to the analysts group. Do not grant `SELECT` on the whole schema if they should only see one table.

3. Use a Unity Catalog column mask on `salary`, or an ABAC column mask policy if this rule must apply across many tagged columns.

4. Use a row filter. The filter UDF can check group membership and row values such as `region`.

5. ABAC. A central governed-tag-based column mask policy is the scalable answer.

6. Usually `USE CATALOG` on `main`, `USE SCHEMA` on `main.silver`, and `MODIFY` on the existing target tables or schema. Add `CREATE TABLE` on the schema only if the job creates new tables.

7. Parent usage privilege. Add `GRANT USE CATALOG ON CATALOG main ...`; also ensure `USE SCHEMA` exists on `main.gold`.

8. Current Databricks docs say `DENY` is not supported for Unity Catalog objects. For UC, use least privilege, `REVOKE`, group design, row filters, or column masks.

9. A column mask must return the same type, or a castable type, as the masked column.

10. A row filter UDF must return `BOOLEAN`; `FALSE` or `NULL` filters out the row.

11. `BROWSE` on the catalog.

12. `DESCRIBE EXTENDED catalog.schema.table` or `DESCRIBE DETAIL catalog.schema.table`; check the table `Type` and `Location`.

## Scenario drills

### Drill 1

A data engineer creates:

```sql
CREATE TABLE main.bronze.raw_orders
USING DELTA
LOCATION 's3://company-raw/orders/';
```

Is this managed or external?

Answer: External, because it has an explicit `LOCATION`.

### Drill 2

A group can read every table in `main.gold`, but should only read `main.gold.revenue`.

Best fix:

```sql
REVOKE SELECT ON SCHEMA main.gold FROM `analysts`;
GRANT SELECT ON TABLE main.gold.revenue TO `analysts`;
```

Also keep:

```sql
GRANT USE CATALOG ON CATALOG main TO `analysts`;
GRANT USE SCHEMA ON SCHEMA main.gold TO `analysts`;
```

### Drill 3

You need to protect all columns tagged `pii = email` in a catalog. New tagged columns should be protected automatically.

Answer: Create an ABAC column mask policy at the catalog or schema level that matches the governed tag.

### Drill 4

A service principal can create a table but cannot query it by name.

Likely issue: missing parent `USE CATALOG` / `USE SCHEMA`, missing `SELECT`, or the object is in a different catalog/schema than expected. Creation privileges do not automatically mean read privileges for every object.
