# Skill: Trino Cluster Health & Query Optimisation

**Domain:** Trino  
**Updated:** 2026-05-20

## When to use
Query plan analysis, OOM diagnosis, worker failures, performance tuning, cluster health scripts.

## Prompt templates

### Optimise a query plan
```
Analyse this Trino EXPLAIN output and suggest optimisations.
Focus on: partition pruning, join reordering, broadcast hints, aggregation pushdown.

[PASTE EXPLAIN OUTPUT]
```

### Diagnose OOM or worker failure
```
Diagnose this Trino worker failure / OOM error.
Suggest config fixes: memory pools, spill-to-disk, session properties.

[PASTE ERROR LOG]
```

### Generate cluster health monitor
```
Write a Python script that checks Trino cluster health via the REST API.
Check: worker count, query queue depth, failed queries in last 15 minutes.
Output a colour-coded status table. Use requests library.
Trino host: [HOST:PORT]
```

### Slow query investigation
```
This Trino query is running slower than expected. Walk me through a diagnosis.
Cluster: [SIZE/CONFIG]
Query runtime: [TIME]
[PASTE QUERY + EXPLAIN ANALYZE OUTPUT IF AVAILABLE]
```

## Quick reference

### REST API health endpoints
```bash
curl http://<trino-host>:8080/v1/cluster
curl http://<trino-host>:8080/v1/query?state=RUNNING
curl http://<trino-host>:8080/v1/node
curl "http://<trino-host>:8080/v1/query?state=FAILED&limit=20"
```

### Key session properties
```sql
SET SESSION query_max_memory = '8GB';
SET SESSION query_max_memory_per_node = '4GB';
SET SESSION spill_enabled = true;
SET SESSION spill_order_by = true;
SET SESSION join_distribution_type = 'AUTOMATIC';
SET SESSION prefer_partial_aggregation = true;
```

### Common failure patterns
| Symptom | Likely cause | Fix |
|---------|-------------|-----|
| OOM on coordinator | Query result too large | Add `LIMIT`, use `spill_enabled` |
| OOM on worker | Skewed join / aggregation | Broadcast hint, salt key |
| Slow partition scan | Missing predicate pushdown | Add partition filter |
| Worker disconnect | GC pressure | Tune JVM `-Xmx`, GC settings |
