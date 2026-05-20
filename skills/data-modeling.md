# Skill: Data Modeling

**Domain:** Architecture / DBT  
**Updated:** 2026-05-20

## When to use
Schema design, dimensional modeling, ERD review, schema evolution, data contract definition,
normalisation trade-offs, and Trino/DBT model structure decisions.

## Prompt templates

### Design a schema from requirements
```
Design a relational schema for this use case.
Target: [Trino / Postgres / BigQuery]
Usage pattern: [read-heavy analytics / transactional / mixed]

Requirements:
- [DESCRIBE ENTITIES AND RELATIONSHIPS]
- Query patterns: [LIST KEY QUERIES]
- Scale: [ROWS, GROWTH RATE]

Output: table definitions with column names, types, constraints, and partition/cluster keys.
Explain normalisation decisions and trade-offs.
```

### Review an existing schema
```
Review this schema design as a Principal Architect.
Check: normalisation level, naming conventions, missing constraints,
partition strategy, data type choices, and query performance implications.
Severity labels: critical / warning / suggestion

[PASTE DDL / SCHEMA DEFINITION]
```

### Design a dimensional model
```
Design a dimensional model (star schema) for this analytics use case.
Target: Trino + DBT

Business process: [e.g. order fulfilment, user activity, pipeline runs]
Grain: [one row per ...]
Key dimensions: [LIST]
Key measures/facts: [LIST]

Output: fact and dimension table definitions, grain statement, SCD strategy for each dimension.
```

### Plan schema evolution
```
Plan a backwards-compatible schema evolution for this change.
Current schema: [PASTE DDL]
Required change: [DESCRIBE — add column / rename / type change / drop]

Output:
1. Migration steps in order
2. DBT model changes required
3. Downstream impact (which models/queries break)
4. Rollback procedure
```

### Define a data contract
```
Define a data contract for this dataset/model.
Include: schema definition, SLA (freshness, latency, availability),
owner, consumers, breaking-change policy, and quality rules.

Dataset: [NAME]
Producer: [TEAM/SERVICE]
Consumers: [LIST]
Usage: [DESCRIBE]
```

### Review DBT model structure
```
Review the structure of these DBT models.
Check: layer separation (staging / intermediate / mart), ref() usage,
grain consistency, missing tests, and naming conventions.

[PASTE MODEL SQL + DAG DESCRIPTION OR LIST OF MODEL NAMES]
```

### Advise on SCD strategy
```
Advise on the right Slowly Changing Dimension strategy for this entity.
Entity: [NAME]
Change frequency: [DAILY / WEEKLY / RARE]
Query needs: [point-in-time / latest-only / full history]
Stack: DBT + Trino

Compare: SCD Type 1, 2, and 3 for this specific case. Recommend one.
```

### Data quality rules design
```
Design data quality rules for this dataset.
Output: dbt test definitions and any custom generic tests needed.

Dataset: [NAME / PASTE SCHEMA]
Business rules: [e.g. order total must be positive, status must be in allowed set]
SLA: [freshness, completeness targets]
```

## Quick reference

### Naming conventions (team standard)
| Object | Convention | Example |
|--------|-----------|---------|
| Staging model | `stg_<source>__<entity>` | `stg_postgres__orders` |
| Intermediate model | `int_<entity>_<verb>` | `int_orders_enriched` |
| Mart / fact | `fct_<business_process>` | `fct_order_fulfilment` |
| Mart / dimension | `dim_<entity>` | `dim_customer` |
| Snapshot | `<entity>_snapshot` | `customer_snapshot` |
| Primary key | `<entity>_id` | `order_id` |
| Timestamps | `<event>_at` (UTC) | `created_at`, `updated_at` |

### Dimensional modeling cheat sheet
| Concept | Rule |
|---------|------|
| Grain | One row = one [event/transaction/day]. State it explicitly. |
| Fact table | Foreign keys + measures. No descriptive attributes. |
| Dimension table | Descriptive attributes. Primary key only. |
| SCD Type 1 | Overwrite. No history. Use for correctable errors. |
| SCD Type 2 | New row per change. Full history. Use for slowly changing attributes. |
| SCD Type 3 | Add column per version. Use only for 1–2 tracked versions. |
| Degenerate dimension | Fact-table attribute with no dimension (e.g. order_number). Keep in fact. |
| Junk dimension | Low-cardinality flags/indicators grouped into one dimension. |

### Trino-specific modeling notes
- Partition by the most common filter column (usually a date)
- Avoid `SELECT *` in production models — column pruning matters at scale
- Use `WITH` (CTE) over subqueries for readability and Trino plan optimization
- Cast types explicitly — Trino is strict about implicit coercion
- For large joins: put the larger table on the left; use broadcast hint for small lookups
