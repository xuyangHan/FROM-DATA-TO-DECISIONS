# The Daily Full Load No Longer Finishes Before Morning

## The Situation

A database extraction originally copied every customer row in one hour. Two years later, the same full load runs past the start of the business day. The team proposes a larger runtime, but most records have not changed since the previous run.

The real opportunity is to stop moving unchanged data.

## Why It Is Not Simple

A **full load** reads and writes the entire dataset. An **incremental load** processes only new or changed data. Incremental processing is faster at scale, but it introduces state and failure modes: missed updates, deletes, late records, and checkpoints advanced too early.

The bottleneck may be the source query, network transfer, transformation, or sink write. Scaling the runtime cannot fix a throttled source or inefficient sink.

## Build the Mental Model

A **watermark** records the last processed value, commonly an update timestamp or increasing ID. The next query requests records beyond that point.

```text
previous watermark < source change value <= captured high watermark
```

Capture the run's high watermark first, process that bounded range, and commit the new watermark only after output succeeds. This prevents new changes that arrive during the run from being skipped.

**Change Data Capture (CDC)** reads inserts, updates, and deletes from database logs or change tables. It handles more change types than a simple timestamp, but requires source support, retention planning, ordering, and operational care.

**Predicate pushdown** sends filters to the source so fewer rows or files are read and transferred. It is valuable only when the source and connector can apply the predicate efficiently.

**Batch** processing handles bounded groups on a schedule. **Streaming** processes events continuously or near real time. Streaming reduces latency but does not remove the need for checkpoints, replay, ordering, and recovery.

## Investigate the Problem

1. Measure source query time, rows read, bytes transferred, transformation time, and sink write time.
2. Check recent data growth, schema changes, indexes, throttling, and retries.
3. Determine how inserts, updates, and deletes can be identified.
4. Confirm the source retains changes long enough for outage recovery.
5. Test whether predicates are pushed down.
6. Reconcile a candidate incremental load against a full snapshot.

## Choose a Solution

| Pattern | Best fit | Main cost |
|---------|----------|-----------|
| Full batch | Small data or periodic reconciliation | Repeated work |
| Watermark batch | Reliable increasing change column | Deletes and late changes |
| CDC batch | Complete row-level changes are required | Source and state complexity |
| Streaming/CDC | Low-latency consumers | Continuous operations |

Many systems use incremental daily processing plus a periodic full reconciliation. This combines efficiency with a way to detect missed changes.

Related reading: [Yesterday's Data Was Loaded Twice](08-yesterdays-data-loaded-twice.md) covers idempotent retries and control tables.

## Production Checklist

- Measure the bottleneck before changing architecture.
- Define how inserts, updates, deletes, and late changes are captured.
- Bound each run and commit state only after durable output.
- Keep enough source history for recovery.
- Make incremental writes idempotent.
- Reconcile counts and business totals periodically.
- Choose streaming only when its latency benefit justifies its operations.

## Interview Takeaways

- Full loads copy everything; incremental loads copy only new or changed data.
- A watermark tracks the last successfully processed change position.
- CDC captures row-level inserts, updates, and deletes.
- Predicate pushdown reduces source reads and transfer.
- Batch processes bounded groups; streaming processes events continuously or near real time.

## Official References

- [Azure Data Factory incremental copy overview](https://learn.microsoft.com/azure/data-factory/tutorial-incremental-copy-overview)
- [Change data capture in SQL Server](https://learn.microsoft.com/sql/relational-databases/track-changes/about-change-data-capture-sql-server)
- [Parquet predicate pushdown](https://parquet.apache.org/docs/file-format/)
