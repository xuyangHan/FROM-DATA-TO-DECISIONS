# Domain 3 — Transformation (practice)

1. Bronze `orders` must become cleaned silver without losing raw history. Write the pattern (read/write), not full code.

2. Fact 2B rows, dimension 10K rows. Join on `product_id`. Optimization?

3. You need all left rows even when dimension missing. Join type?

4. Stack `events_2024` and `events_2025` with slightly different columns. Safer union API?

5. Remove duplicate `order_id` keeping arbitrary first row. Method?

6. Billion-row `user_id` cardinality for dashboard approximate DAUs. Function?

7. Spark UI shows one task 20× slower in shuffle stage. Problem name and one fix?

8. Gold layer: executives need pre-aggregated daily revenue queried 100×/day; data updated hourly. Table, view, MV, or streaming table?
